---
layout: default
---

# [](#我的app crash了，怎么办？——1)我的appcrash了，怎么办？——1

> 这篇文章的作者是iOS Tutorial Team 的成员[Matthijs Hollemans](https://www.raywenderlich.com/about#matthijshollemans),他是一个经验丰富的ios设计开发工作者，他的联系方式：[Google+]() 和 [Twitter](),[原文链接](https://www.raywenderlich.com/10209/my-app-crashed-now-what-part-1)

我们大多数开发人员经常遇到这样的情况：我们的应用运行的好好的，突然——“砰”一下子crash了，抓狂！
别慌！
假如此时很郁闷的你立即开始尝试修改代码，并期望如同寻找到了合适的咒语一样使得bug神奇消失，那么很可能导致更糟糕的问题。但是如果掌握了系统的定位crash问题的方法的话，解决crash问题也不是很复杂的事情。

首要的事情，是找到代码中crash出现的确切位置：哪个文件的哪行代码。本文将全面的阐述如何利用Xcode的调试工具定位代码的奔溃位置。

本文面向所有的开发者，从初级到高级。即便是高级开发者，也可能通过本文获得一些调试技巧或者以前不曾涉及的调试知识。

## [](#一、准备工作)一、准备工作

下载[示例程序](http://www.raywenderlich.com/downloads/Problems.zip).这是个有bug的程序。用Xcode打开，可以看到有至少八处编译警告，通常编译警告也是问题的前期表现。本文中用的Xcode4.3来做说明，但实际上在4.2的版本也是一样的。

>**注：** 本文的示例程序效果是在IOS5的模拟器上运行的效果，如果直接在手机上运行，同样也会crash，但是crash出现的顺序可能会有所不同

在模拟器上运行看看发生了什么情况。

![第一个crash](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/First-crash.png  "第一个crash")

嗯，代码crash了。:-）

通常crash分两种：SIGABRT (或者 EXC_CRASH) 和 EXC_BAD_ACCESS (通常用 SIGBUS 或 SIGSEGV表示的crash)。

SIGABRT的Crash通常情况下好定位很多，因为它是受控的crash（系统让app去执行某个app本身并不支持的操作时，应用终端就会直接抛出该信号，让程序crash）。

EXC_BAD_ACCESS的crash定位就要难很多。这种crash经常发生在应用进入了一个损坏状态时，通常是由内存管理问题导致。

幸运的是，我们上面的第一个crash（目前暴露出来的）是SIGABRT。SIGABRT的crash发生时，一般在Xcode的调试输出窗口（窗口的右下角）会有错误信息输出。如果你看不到调试输出窗口，通过View—》Debug Area-》Show Debug Area显示调试输出区域。此处，这个crash的错误信息如下：

```
Problems[14465:f803] -[UINavigationController setList:]: unrecognized selector sent to
instance 0x6a33840
Problems[14465:f803] *** Terminating app due to uncaught exception 'NSInvalidArgumentException',
reason: '-[UINavigationController setList:]: unrecognized selector sent to instance 0x6a33840'
*** First throw call stack:
(0x13ba052 0x154bd0a 0x13bbced 0x1320f00 0x1320ce2 0x29ef 0xf9d6 0x108a6 0x1f743
0x201f8 0x13aa9 0x12a4fa9 0x138e1c5 0x12f3022 0x12f190a 0x12f0db4 0x12f0ccb 0x102a7
0x11a9b 0x2792 0x2705)
terminate called throwing an exception
```

学会解析这些错误信息很关键。因为这些信息中往往包含了很重要的错误原因描述。比如上面输出中比较有趣的一行：

```
[UINavigationController setList:]: unrecognized selector sent to instance 0x6a33840
```

错误信息“unrecognized selector sent to instance XXX”的意思是说应用尝试调用一个不存在的方法。通常是由调用方法的对象不正确导致。此处有问题的对象是一个UINavigationController对象（内存地址为0x6a33840），调用的方法是setList:。

知道了crash的原因就好了，当前的首要任务就是找出代码中出错的地方。必须找到源文件名字以及出错行数。可以借助调用堆栈（also known as the stacktrace or the backtrace）。

当一个应用crash，Xcode窗口左侧的区域会切换进入调试导航页。显示出当前应用中活动状态的线程信息，并高亮标出crash掉的线程。 通常都是应用的主线程Tread 1，因为大部分的业务工作在这个线程中完成的。如果应用中使用了队列或后台线程，崩溃也会出现在这些线程中。

![堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Call-stack-compressed.png  "堆栈信息")

目前，Xcode会自动标出出错点在**main.m**文件中的**main()**函数中。此处并不会提供更多信息，所以我们必须更深入一些寻找线索。

查看更多的堆栈信息，往右侧拖动堆栈信息下方的滑块，这样将会完整显示当前crash掉的线程信息：

![堆栈完整信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Call-stack-expanded.png  "堆栈完整信息")

列表中的每一条都是一个应用中或者某一个IOS frameworks的函数或方法。堆栈信息能显示出应用中当前还处于活动状态的方法和函数。调试器暂停了应用中断了这些函数和方法的执行。

最后一条函数**start()**是入口。Somewhere in its execution it called the function above it, main()。是应用的入口点，经常显示在靠近底部的位置。**main()** 调用了 **UIApplicationMain()**，就是编辑窗口中绿色箭头指向的代码行。

Going further up the stack, UIApplicationMain() called the _run method on the UIApplication object, which called CFRunLoopRunInMode(), which called CFRunLoopRunSpecific(), and so on, all the way up to __pthread_kill.

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Call-stack-principle.png  "调用堆栈信息")

All of these functions and methods in the call stack, except for main(), are grayed out. That’s because they come from the built-in iOS frameworks. There is no source code available for them.
The only thing in this stacktrace that you have source code for is main.m, so that’s what the Xcode source editor shows, even though it’s not really the true source of the crash. This often confuses new developers, but in a minute I will show you how to make sense of it.
For fun, click on any one of the other items from the stacktrace and you’ll see a bunch of assembly code which might not make much sense to you:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Assembly-code.png  "调用堆栈信息")

Oh, if only we had the source code for that! :-）

## [](#二、异常断点)二、异常断点

So how do you find the line in the code that made the app crash? Well, whenever you get a stacktrace like this, an exception was thrown by the app. (You can tell because one of the functions in the call stack is named objc_exception_rethrow.)
An exception happens when the program is caught doing something it shouldn’t have done. What you’re looking at now is the aftermath of this exception: the app did something wrong, the exception has been thrown, and Xcode shows you the results. Ideally, you’d want to see exactly where that exception gets thrown.
Fortunately, you can tell Xcode to pause the program at just that moment, using an Exception Breakpoint. A breakpoint is a debugging tool that pauses your program at a specific moment. You’ll see more of them in the second part of this tutorial, but for now you’ll use a specific breakpoint that will pause the program just before an exception gets thrown.
To set the Exception Breakpoint, we have to switch to the Breakpoint Navigator:

![断点导航](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Breakpoint-navigator.png  "断点导航")

At the bottom is a small + button. Click this and select Add Exception Breakpoint:

![添加断点](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Add-exception-breakpoint.png  "添加断点")

A new breakpoint will be added to the list:

![添加好的断点](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Exception-breakpoint-added.png  "添加好的断点")

Click the Done button to dismiss the pop-up. Notice that the Breakpoints button in Xcode’s toolbar is now enabled. If you want to run the app without any breakpoints enabled, you can simply toggle this button to off. But for now, leave it on and run the app again.

![崩溃中的高亮代码](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Crash-with-source-line-highlighted.png  "崩溃中的高亮代码")

That’s better! The source code editor now points to a line from the source code – no more nasty assembly stuff – and notice that the call stack on the left (you might need to switch to the call stack via the Debug Navigator depending on how you have Xcode set up) also looks different.
Apparently, the culprit is this line in the AppDelegate’s application:didFinishLaunchingWithOptions: method:

```viewController.list = [NSArray arrayWithObjects:@"One", @"Two"];```

Take a look at that error message again:

```[UINavigationController setList:]: unrecognized selector sent to instance 0x6d4ed20```

In the code, “viewController.list = something” calls setList: behind the scenes, because “list” is a property on the MainViewController class. However, according to the error message the viewController variable does not point to a MainViewController object but to a UINavigationController – and of course, UINavigationController does not have a “list” property! So things are getting mixed up here.
Open the Storyboard file to see what the window’s rootViewController property actually points to:

![Storyboard-with-navigation-controller](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Storyboard-with-navigation-controller.png  "Storyboard-with-navigation-controller")

Ah ha! The storyboard’s initial view controller is a Navigation Controller. That explains why window.rootViewController is a UINavigationController object instead of the MainViewController that you’d expect. To fix this, replace application:didFinishLaunchingWithOptions: with the following:

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
	UINavigationController *navController = (UINavigationController *)self.window.rootViewController;
	MainViewController *viewController = (MainViewController *)navController.topViewController;
	viewController.list = [NSArray arrayWithObjects:@"One", @"Two"];
	return YES;
}
```

First you get a reference to the UINavigationController from self.window.rootViewController, and once you have that you can get the pointer to the MainViewController by asking the navigation controller for its topViewController. Now the viewController variable should point to the proper object.

>**Note:** Whenever you get a “unrecognized selector sent to instance XXX” error, check that the object is of the right type and that it actually has a method with that name. Often you’ll find that you’re calling a method on a different object than you thought, because a pointer variable may not contain the right value.
> ![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Unrecognized-selector-parts.png  "调用堆栈信息")
> Another common reason for this error is a misspelling of the method name. You’ll see an example of this in a bit.

## [](#三、你的第一个内存错误)三、你的第一个内存错误

That should have fixed our first problem. Run the app again. Whoops, it crashes at the same line, only now with an EXC_BAD_ACCESS error. That means the app has a memory management problem.

![EXC_BAD_ACCESS-on-array](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/EXC_BAD_ACCESS-on-array.png  "EXC_BAD_ACCESS-on-array")

The source of a memory-related crash is often hard to pinpoint, because the evil may have been done much earlier in the program. If a malfunctioning piece of code corrupts a memory structure, the results of this may not show up until much later, and in a totally different place.
In fact, the bug may never show up for you at all while testing, and only rear its ugly head on the devices of your customers. You don’t want that to happen!
This particular crash, however, is easy to fix. If you look at the source code editor, Xcode has been warning you about this line all along. See the yellow triangle on the left next to the line numbers? That indicates a compiler warning. If you click on the yellow triangle, Xcode should pop up a “Fix-it” suggestion like this:

![Missing-sentinel](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Missing-sentinel.png  "Missing-sentinel")

The code initializes an NSArray object by giving it a list of objects, and such lists are supposed to be terminated using nil, the sentinel referred to in the warning. But that wasn’t done and now NSArray gets confused. It tries to read objects that don’t exist, and the app crashes hard.
This is a mistake you really shouldn’t make, especially since Xcode already warns you about it. Fix the code by adding nil to the list as follows (Or, you can simply select the “Fix-it” option from the menu):

```viewController.list = [NSArray arrayWithObjects:@"One", @"Two", nil];```

## [](#四、“This class is not key value coding-compliant”)四、“This class is not key value coding-compliant”

Run the app again to see what other fun bugs this project has in store for you. And what do you know? It crashes again on main.m. Since the Exception Breakpoint is still enabled and we do not see any app source code highlighted, this time the crash truly didn’t happen in any of the app source code. The call stack corroborates this: none of these methods belong to the app, except main():

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-crash.png  "调用堆栈信息")

If you look through the method names from the top going down, there is some stuff going on with NSObject and Key-Value Coding. Below that is a call to [UIRuntimeOutletConnection connect]. I have no idea what that is, but it looks like it has something to do with connecting outlets. Below that are methods that talk about loading views from a nib. So that gives you some clues already.
However, there is no convenient error message in Xcode’s Debug Pane. That’s because the exception hasn’t been thrown yet. The Exception Breakpoint has paused the program just before it tells you the reason for the exception. Sometimes you get a partial error message with the Exception Breakpoint enabled, and sometimes you don’t.
To see the full error message, click the “Continue Program Execution” button in the debugger toolbar:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Continue-program-execution-button.png  "调用堆栈信息")

You may need to click it more than once, but then you’ll get the error message:

```
Problems[14961:f803] *** Terminating app due to uncaught exception 'NSUnknownKeyException', 
reason: '[<MainViewController 0x6b3f590> setValue:forUndefinedKey:]: this class is not
key value coding-compliant for the key button.'
*** First throw call stack:
(0x13ba052 0x154bd0a 0x13b9f11 0x9b1032 0x922f7b 0x922eeb 0x93dd60 0x23091a 0x13bbe1a 
0x1325821 0x22f46e 0xd6e2c 0xd73a9 0xd75cb 0xd6c1c 0xfd56d 0xe7d47 0xfe441 0xfe45d 
0xfe4f9 0x3ed65 0x3edac 0xfbe6 0x108a6 0x1f743 0x201f8 0x13aa9 0x12a4fa9 0x138e1c5 
0x12f3022 0x12f190a 0x12f0db4 0x12f0ccb 0x102a7 0x11a9b 0x2872 0x27e5)
terminate called throwing an exception
```
As before, you can ignore the numbers at the bottom. They represent the call stack, but you already have that in a more convenient – and readable! – format in the Debug Navigator on the left.
The interesting bits are:

* NSUnknownKeyException
* MainViewController
* “this class is not key value coding-compliant for the key button”

The name of the exception, NSUnknownKeyException, is often a good indicator of what is wrong. It tells you there is an “unknown key” somewhere. That somewhere is apparently MainViewController, and the key is named “button.”
As we’ve already established, all of this happens while loading the nib. The app uses a storyboard rather than nibs, but internally the storyboard is just a collection of nibs, so it must be a mistake in the storyboard.
Check out the outlets on MainViewController:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-outlet-in-storyboard.png  "调用堆栈信息")

In the Connections Inspector, you can see that the UIButton in the center of the view controller is connected to MainViewController’s “button” outlet. So the storyboard/nib refers to an outlet named “button,” but according to the error message it can’t find this outlet.
Have a look at MainViewController.h:

```
@interface MainViewController : UIViewController

@property (nonatomic, retain) NSArray *list;
@property (nonatomic, retain) IBOutlet UIButton *button;

- (IBAction)buttonTapped:(id)sender;

@end
```
The @property definition for the “button” outlet is there, so what’s the problem? If you’ve been paying attention to the compiler warnings, you may have figured it out already.
If not, check out MainViewController.m/s @synthesize list. Do you see the problem now?
The code doesn’t actually @synthesize the button property. It tells MainViewController that it has a property named “button,” without providing it with a backing instance variable and getter and setter methods (which is what @synthesize does).
Add the following to MainViewController.m below the existing @synthesize line to fix this issue:

```
@synthesize button = _button;
```
Now the app should no longer crash when you run it!

>**Note: **The error “this class is not key value coding-compliant for the key XXX” usually occurs when loading a nib that refers to a property that doesn’t actually exist. This usually happens when you remove an outlet property from your code but not from the connections in the nib.

## ()[#Push the Button]Push the Button

Now that the app works – or at least starts up without problems –, it’s time to tap that button.

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/App-with-button.png  "调用堆栈信息")

Woah! The app crashes with a SIGABRT on main.m. The error message in the Debug Pane is:

```
Problems[6579:f803] -[MainViewController buttonTapped]: unrecognized selector sent
to instance 0x6e44850
```
The stack trace isn’t too illuminating. It lists a whole bunch of methods that are related one way or the other to sending events and performing actions, but you already know that actions were involved. After all, you tapped a UIButton and that results in an IBAction method being called.
Of course, you’ve seen this error message before. A method is being called that does not exist. This time the target object, MainViewController, looks to be the right one since action methods usually live in the view controller that contains the buttons. And if you look in MainViewController.h, the IBAction method is there indeed:

```
- (IBAction)buttonTapped:(id)sender;
```

Or is it? The error message says the method name is buttonTapped, but MainViewController has a method named buttonTapped:, with a colon at the end because this method accepts one parameter (named “sender”). The method name from the error message, on the other hand, does not include a colon and therefore takes no parameters. The signature for that method looks like this instead:

```
- (IBAction)buttonTapped;
```
What happened here? The method initially did not have a parameter (something that is allowed for action methods), at which time the connection in the storyboard was made to the button’s Touch Up Inside event. However, some time after that, the method signature was changed to include the “sender” parameter, but the storyboard was not updated.
You can see this in the storyboard, on the Connections Inspector for the button:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-connections.png  "调用堆栈信息")

First disconnect the Touch Up Inside event (click the small X), then connect it to the Main View Controller again, but this time select the buttonTapped: method. Notice that in the Connections Inspector there is now a colon after the method name.
Run the app and tap the button again. What the?! Again you get the “unrecognized selector” message, although this time it correctly identifies the method as buttonTapped:, with the colon.

```
Problems[6675:f803] -[MainViewController buttonTapped:]: unrecognized selector sent
to instance 0x6b6c7f0
```
If you look closely, the compiler warnings should point you to the solution again. Xcode complains that the implementation of MainViewController is incomplete. Specifically, the method definition for buttonTapped: is not found.

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Incomplete-implementation-warning.png  "调用堆栈信息")

Time to look at MainViewController.m. There certainly appears to be a buttonTapped: method in there, although… wait a minute, it’s spelled wrong:

```
- (void)butonTapped:(id)sender
```
Easy enough to fix. Rename the method to:

```
- (void)buttonTapped:(id)sender
```
Note that you don’t necessarily need to declare it as IBAction, although you can do so if you think that is neater.

>**Note: **This sort of thing is easy to catch if you’re paying attention to the compiler warnings. Personally, I treat all warnings as fatal errors (there is even an option for this in the Build Settings screen in Xcode) and I’ll fix each and every one of them before running the app. Xcode is pretty good at pointing out silly mistakes such as these, and it’s wise to pay attention to these hints.

## [](#Messing with Memory)Messing with Memory

You know the drill: run the app, tap the button, wait for the crash. Yep, there it is:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/NSLog-crash.png  "调用堆栈信息")

It’s another one of those EXC_BAD_ACCESS ones, yikes! Fortunately, Xcode shows you exactly where the crash happened, in the buttonTapped: method:

```
NSLog("You tapped on: %s", sender);
```

Sometimes these mistakes may take a moment or two to register in your mind, but again Xcode lends a helping hand – just tap the yellow triangle to see what’s wrong:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Insert-@.png  "调用堆栈信息")

NSLog() takes an Objective-C-type string, not a plain old C-string, so inserting an @ will fix it:

```
NSLog(@"You tapped on: %s", sender);
```

You’ll notice that the warning yellow triangle doesn’t go away. This is because this line has another bug that may or may not crash your app. Those are the fun ones. Sometimes the code works just fine – or at least appears to work just fine – and at other times it will crash. (Of course, these sorts of crashes only happen on customers’ devices, never on your own.)

Let’s see what the new warning is:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Format-string-issue.png  "调用堆栈信息")

The %s specifier is for C-style strings. A C-string is just a section of memory – a plain old array of bytes – that is terminated by a so-called “NUL character,” which is really just the value 0. For example, the C-string “Crash!” looks like this in memory:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/C-string.png  "调用堆栈信息")

Whenever you use a function or method that expects a C-style string, you have to make sure the string ends with the value 0, or the function will not recognize that the string has ended.
Now, when you specify %s in an NSLog() format string – or in NSString’s stringWithFormat – then the parameter is interpreted as if it were a C-string. In this case, “sender” is the parameter, and it’s a pointer to a UIButton object, which is definitely not a C-string. If whatever “sender” points to contains a 0 byte, then the NSLog() will not crash, but output something such as:

```
You tapped on: xËj
```

You can actually see where this comes from. Run the app again, tap the button and wait for the crash. Now, in the left half of the Debug Pane, right-click on “sender” and pick the “View Memory of *sender” option (make sure to choose the one with the asterisk in front of sender).

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/View-memory-menu.png  "调用堆栈信息")

Xcode will now show you the memory contents at that address, and it’s exactly what NSLog() printed out.

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Memory-contents.png  "调用堆栈信息")

However, there is no guarantee there is a NUL byte, and you could just as easily run into a EXC_BAD_ACCESS error. If you’re always testing your app on the simulator that may not happen for a long time, as the circumstances may always be in your favor in your particular testing environment. That makes these kinds of bugs very hard to trace.
Of course, in this case Xcode has already warned you about the wrong format specifier, so this particular bug was easy to find. But whenever you’re using C-strings or manipulating memory directly, you have to be very careful not to mess around with someone else’s memory.
If you’re lucky the app will always crash and the bug is easy to find, but more commonly, the app will only crash sometimes – making the problem hard to reproduce! – and then the hunt for the bug can take on epic proportions.
Fix the NSLog() statement as follows:

```
NSLog(@"You tapped on: %@", sender);
```

Run the app and press the button once more. The NSLog() does what it’s supposed to, but looks as if you’re not done crashing in buttonTapped: yet.

## [](#Making Friends With the Debugger)Making Friends With the Debugger

For this latest crash, Xcode points at the line:

```[self performSegueWithIdentifier:@"ModalSegue" sender:sender];```

There is no message in the Debug Pane. You can press the Continue Program Execution button like you did before, but you can also type a command in the debugger to get the error message. The advantage of doing this is that the app can stay paused in the same place.
If you’re running this from the simulator, you can type the following after the (lldb) prompt:

```(lldb) po $eax```

LLDB is the default debugger for Xcode 4.3 and up. If you’re using an older version of Xcode, then you have the GDB debugger. They share some basic commands, so if your Xcode prompt says (gdb) instead of (lldb), you should still be able to follow along without a problem. (By the way, you can switch between debuggers in the Scheme editor in Xcode, under the Run action. And you can access the Scheme editor by Alt-tapping the Run icon at the top left corner of your Xcode window.)
The po command stands for “print object.” The symbol $eax refers to one of the CPU registers. In the case of an exception, this register will contain a pointer to the NSException object. Note: $eax only works for the simulator, if you’re debugging on the device you’ll need to use register $r0.
For example, if you type:

```(lldb) po [$eax class]```

You will see something like this:

```(id) $2 = 0x01446e84 NSException```

The numbers aren’t important, but it’s obvious you’re dealing with an NSException object here.
You can call any method from NSException on this object. For example:

```(lldb) po [$eax name]```

This will give you the name of the exception, in this case NSInvalidArgumentException, and:

```(lldb) po [$eax reason]```

This will give you the error message:

```
(unsigned int) $4 = 114784400 Receiver (<MainViewController: 0x6b60620>) has no
segue with identifier 'ModalSegue'
```


>Note: When you just do “po $eax”, it will call the “description” method on the object and print that, which in this case also gives you the error message.


So that explains what’s going on: you’re attempting to perform a segue named “ModalSegue” but apparently there is no such segue on the MainViewController.

The storyboard does show that a segue is present, but you’ve forgotten to set its identifier, a typical mistake:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Segue-identifier.png  "调用堆栈信息")

Change the segue identifier to “ModalSegue.” Run the app again, and – wait for it – tap the button. Whew, no more crashes this time! But here’s a teaser for our next part – the table view that shows up isn’t supposed to be empty!

## []()Where to Go From Here?

So what about that empty table? I’m going to hold you in suspense for now. You’ll tackle it in Part Two of this tutorial, along with some more fun bugs you’re likely to come across in your coding life. Also in Part Two, you’ll add some more tools to your debugging arsenal, including the NSLog() statement, breakpoints and Zombie Objects.
When all is said and done, I promise that the app will run as its supposed to! More importantly, you’ll have accrued skills for when you run into these frustrations in your own apps – as you inevitably will.
Any questions or comments so far? Let me know in the forums!

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/hollemans_100x100.jpg  "调用堆栈信息")This is a post by iOS Tutorial Team member Matthijs Hollemans, an experienced iOS developer and designer. You can find him on Google+ and Twitter.





