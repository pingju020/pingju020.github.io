---
layout: default
---

# [](#我的app崩溃了，怎么办？——1)我的appcrash了，怎么办？——1

> 这篇文章的作者是iOS Tutorial Team 的成员[Matthijs Hollemans](https://www.raywenderlich.com/about#matthijshollemans),他是一个经验丰富的ios设计开发工作者，他的联系方式：[Google+](https://plus.google.com/111394725745201507398?rel=author) 和 [Twitter](https://twitter.com/mhollemans)

>阅读原文：[原文链接](https://www.raywenderlich.com/10209/my-app-crashed-now-what-part-1)

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

最后一条函数**start()**是入口。这个函数执行的过程中调用了它上面的函数, **main()**。是应用的入口点，经常显示在靠近底部的位置。**main()** 调用了 **UIApplicationMain()**，就是编辑窗口中绿色箭头指向的代码行。

查看更深入的堆栈信息，**UIApplicationMain()** 调用了 **UIApplication** 对象的 **_run** 方法。**_run**调用了**CFRunLoopRunInMode()**，**CFRunLoopRunInMode()**又调用了**CFRunLoopRunSpecific()**，这样层层调用直到 **__pthread****_kill**。

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Call-stack-principle.png  "调用堆栈信息")

除了**main()**，调用堆栈中所有的函数和方法全部是灰化显示。这是因为这些信息都来自编译好的iOS库，没有有效的源代码导致。

堆栈中的源代码文件只有**main.m**，所以尽管**main.m**并不是真正引入crash的文件，但是Xcode文件编辑器提示崩溃的点还是在这个源文件里。这个经常弄迷糊新手，所以本文中将快速给出一个方便大家理解的途径。

点击堆栈信息中其他的任何一条信息，都能看到一堆毫无头绪的汇编代码：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Assembly-code.png  "调用堆栈信息")

哦，如果有源代码就好了！ :-）

## [](#二、异常断点)二、异常断点

因此到底该如何找到crash的代码行呢？不论何时出现了上面的堆栈信息的时候，都是app抛出了异常。（也可以说是堆栈上调用到了**objc_exception_rethrow**函数）应用做了不该做的事情就会抛出异常。目前关注的是这个异常导致的结果：app做了些不应该做的事情，抛出了异常，Xcode将其呈现出来。我们想确切知道到底是哪里抛的异常。

幸运的是，Xcode还可以打全局断点暂停程序。断点可以帮助开发人员在某个场景暂停程序执行，本文的第二部分将会详细阐述这块。这里需要使用的是一种特殊的断点——全局断点，程序crash前进入全局断点。

可以进入断点导航页设置全局断点：

![断点导航](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Breakpoint-navigator.png  "断点导航")

点击底部的小**+** 按钮,选择**Add Exception Breakpoint**：

![添加断点](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Add-exception-breakpoint.png  "添加断点")

新的全局断点就添加好了:

![添加好的断点](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Exception-breakpoint-added.png  "添加好的断点")

点击确定按钮退出弹框提醒。此时Xcode的工具栏的断点按钮现在变成可用状态。如果你想程序跑起来不进任何断点的话，可以点击这个断点按钮关闭断点，但是现在，打开断点并运行app。

![崩溃中的高亮代码](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Crash-with-source-line-highlighted.png  "崩溃中的高亮代码")

好了，代码编辑器现在不再是不再是汇编代码，而是指向了源代码，同时注意看左侧的堆栈信息（是否切换出来堆栈信息，由Xcode的设置决定）也发生了变化。

明显，问题指向**AppDelegate**的**application:didFinishLaunchingWithOptions:**方法中下面这行代码：

```viewController.list = [NSArray arrayWithObjects:@"One", @"Two"];```

再来看看错误信息:

```[UINavigationController setList:]: unrecognized selector sent to instance 0x6d4ed20```

代码中的“viewController.list = something”调用了**setList:**，因为“list”是MainViewController类的一个属性。尽管在错误处看viewController并不是指向MainViewController对象的实例而是指向了UINavigationController，当然UINavigationController根本没有一个“list”属性。又混乱了！

打开Storyboard查看窗口的rootViewController属性实际是这样：

![Storyboard-with-navigation-controller](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Storyboard-with-navigation-controller.png  "Storyboard-with-navigation-controller")

啊哈！storyboard的初始控制视图是Navigation Controller。这样就能解释为什么为什么window.rootViewController指向UINavigationController对象而不是预计的MainViewController对象。 用下面的代码替换**application:didFinishLaunchingWithOptions:**来处理：

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
	UINavigationController *navController = (UINavigationController *)self.window.rootViewController;
	MainViewController *viewController = (MainViewController *)navController.topViewController;
	viewController.list = [NSArray arrayWithObjects:@"One", @"Two"];
	return YES;
}
```

先通过self.window.rootViewController获取到UINavigationController的引用，接着将navigation controller的topViewController设置成MainViewController的指针。现在，viewController就指向正确的对象了。

>**注:** 一旦出现 “unrecognized selector sent to instance XXX” 错误, 首先检查对象的类型是不是正确以及被调用的方法究竟是否存在。经常遇到的情况是指针并没有指向正确的值导致实际调用的压根就是预期外的其他类型对象的方法。
> ![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Unrecognized-selector-parts.png  "调用堆栈信息")
> 另外，方法名拼写错误也会导致出现该问题。稍后将会给出一个这样的示例程序。

## [](#三、你的第一个内存错误)三、你的第一个内存错误

第一个问题修复了，再来运行应用看看。 喔，又在同一行Crash了, 只不过这次是 EXC_BAD_ACCESS 错误。看样子，该应用还有内存处理上的问题。

![EXC_BAD_ACCESS-on-array](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/EXC_BAD_ACCESS-on-array.png  "EXC_BAD_ACCESS-on-array")

定位内存相关的crash相对来说会比较困难，因为隐患可能出现在崩溃前已经运行很久的其他代码中。有问题的代码破坏了内存结构，并不会立即在程序中体现出来，而是到一段时间后，其他地方再访问这段内存有问题了，才会爆出crash出来。

而且事实上，可能测试的时候这种bug根本就不会暴露出来，最终往往暴露在用户的手机上。谁也不想出现这种情况。然而这种类型的crash也是比较容易处理的。如果仔细查看代码编辑框就会发现，Xcode原来早就警告我们这些存在内存处理不妥当的代码行了。看见代码左侧行标上的黄色三角形了么？那就是一个编译警告。点击黄色的三角形，Xcode会自动弹出一个“Fix-it”的修复建议，如下图：

![Missing-sentinel](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Missing-sentinel.png  "Missing-sentinel")

此处用一种对象的序列来初始化NSArray对象，这种序列需要以nil结尾，但是此处并没有以nil结尾，编译器不知道该何处是序列的结尾，所以就报了这个警告出来。运行时，因为没有明显的结尾标志，所以系统会在读完了所有的参数后，还尝试获取并不存在的对象，添加到序列中，所以就崩溃了。

这种错误实在不应该犯，特别是Xcode已经给出警告后。修复这个bug可以像下面代码一样，给序列添加nil结尾项（或者，直接点击“Fix-it”）：

```viewController.list = [NSArray arrayWithObjects:@"One", @"Two", nil];```

## [](#四、“这个类不符合键值编码”)四、“这个类不符合键值编码”

在运行代码，看还有哪些其他有趣的bug。但是你是怎么知道的？它又一次崩溃在了**main.m**中。尽管全局断点还有效我们却看不到任何高亮的代码提示，这次代码的crash实实在在没有发生在我们应用的源代码中。堆栈信息证实，除了**main()**，没有一个方法属于我们的应用：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-crash.png  "调用堆栈信息")

自上而下查看方法名，可以发现有些事情发生跟**NSObject**和**键值编码(Key-Value Coding)**有关系。其下，是对**[UIRuntimeOutletConnection connect]**的调用。不知道该怎么办，但是看上去好像跟**绑定(connect outlets)**有关系.再往下调用的方法是从nib中加载view。这已经给出了线索。然而Xcode的调试窗还没有很方便定位的错误信息，因为系统尚未抛出异常。全局断点仅仅会在程序告诉你异常原因前中断程序。有时候你会获取到一个明确的错误信息，有时候并不能获取到。

想看到完整的错误信息，点击调试窗口工具栏的**“Continue Program Execution”**按钮：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Continue-program-execution-button.png  "调用堆栈信息")

有时候可能需要多点几下，才能看到打印出来的错误信息：

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

和之前一样忽略下方的数字。它们表示的是调用栈信息，但是我们已经有关于它们的更方便和可读的信息了！就是左侧的调试导航页面。

有趣的信息如下:

* NSUnknownKeyException
* MainViewController
* “this class is not key value coding-compliant for the key button”

异常的名字**NSUnknownKeyException**通常能很好的指示出问题原因。比如此处，就告诉我们代码某处使用了系统不知道的**“键值（unknown key）”**。这里的某处很明显是**MainViewController**，而且键值名应该就是**“button”**。

可以确定，问题就发生在加载nib的时候。虽然应用直接使用的是storyboard，但是更深入些storyboard实际就是所有nib的集合，所以问题应该就处在storyboard中。

检查**MainViewController**中的所有**outlet**：


![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-outlet-in-storyboard.png  "调用堆栈信息")

在链接监测区，可以看到试图控制器中心的UIButton被链接到**MainViewController**的**“button”**了。所以**storyboard/nib**有一个出口叫**“button”**，但是根据错误信息看的话，实际根本没有这个出口。

看看**MainViewController.h**:

```
@interface MainViewController : UIViewController

@property (nonatomic, retain) NSArray *list;
@property (nonatomic, retain) IBOutlet UIButton *button;

- (IBAction)buttonTapped:(id)sender;

@end
```

此处@property 定义了名为 “button” 出口, 所以到底是怎么回事? 如果你注意到编译警告，或许就不难找到问题症结了。

即使没有，检查下**MainViewController.m**文件中的**@synthesiz**列表。现在找到问题了么？

代码并没有准确的**@synthesize**按钮属性。它告诉**MainViewController**有一个名字叫**“button”**的属性，但是却并没有提供实例变量以及存取方法(这些都是由**@synthesize**完成的)

在**MainViewController.m**中的**@synthesize**之下添加如下代码来处理这个问题：

```
@synthesize button = _button;
```
现在运行程序将不再crash了！

>**注:** “this class is not key value coding-compliant for the key XXX” 错误经常出现在加载声明但是并未实现的属性的nib时。一般当从源文件中删除了一个outlet属性，但是并没有从nib去掉队形链接时，就会出现这中错误。

## [](#五、点击按钮)五、点击按钮

现在应用可以运行了，或者说至少启动没问题了，来，点击按钮试试。

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/App-with-button.png  "调用堆栈信息")

哇哦，应用崩溃在**main.m**中，还报了个**SIGABRT**错误信息。调试窗口的错误信息如下：

```
Problems[6579:f803] -[MainViewController buttonTapped]: unrecognized selector sent
to instance 0x6e44850
```

堆栈信息并不很明了，它列出了所有的可能通过这样那样途径发消息或执行操作的方法，但是已经可以知道哪个操作有问题。毕竟这里是点击了UIButton，调用IBAction method时出的问题。

当然，之前已经遇到过类似问题了。因为调用了一个并没有实现的方法导致。不过这次目标对象**MainViewController**似乎没有问题，因为活动方法就是在这个有按钮的view controller中。而且头文件**MainViewController.h**中也确实存在IBAction方法：

```
- (IBAction)buttonTapped:(id)sender;
```

或者是不是这样？错误信息是想告诉我们方法名是**buttonTapped**，但是**MainViewController**的方法名却是以冒号结尾的**buttonTapped:**，因为它允许传入一个参数(名字叫**“sender”**)。反过来说，错误信息中的方法名并不包含冒号，因为不需要传入参数。所以正确格式的方法应该是这样：

```
- (IBAction)buttonTapped;
```

这里到底是怎么回事呢？方法初始化的时候是没有参数的格式（有些情况允许没有参数的响应方法），同时，storyboard将该方法关联成了按钮的点击（Touch Up Inside）事件响应。但是，后来方法变成了包含一个“sender”参数的格式，但是storyboard的关联没有实时更新。

我们可以在storyboard的按钮链接窗口看到如下场景：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Button-connections.png  "调用堆栈信息")

先断开点击(Touch Up Inside)的链接（点击小的X按钮），接着将其再一次链接到主视图控制器，不过这次选择**buttonTapped:**方法。注意，这时链接窗口中的方法名末尾是包含了冒号的。

再运行程序后点击按钮。什么鬼？尽管现在已经使用了有冒号的正确格式的点击方法**buttonTapped:**，还是报**“unrecognized selector”**错误信息，

```
Problems[6675:f803] -[MainViewController buttonTapped:]: unrecognized selector sent
to instance 0x6b6c7f0
```

如果仔细查看的话，就会发现编译警告又是跟上面类似的场景。Xcode在抱怨**MainViewController**实现文件不完整。特别是**buttonTapped:**没有实现。

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Incomplete-implementation-warning.png  "调用堆栈信息")

该看看**MainViewController.m**文件了。这里面明明有**buttonTapped:**方法的，呃，等等，拼写好像不对：

```
- (void)butonTapped:(id)sender
```

好了这个很好修复，修改下名字:


```
- (void)buttonTapped:(id)sender
```

请注意，尽管将方法定义成IBAction会使代码看上去整洁，但是也没有必要非将方法定义成IBAction。

>**注:**如果留神编译器的警告的话，这章节的问题都是比较容易定位的。就个人来说，我是将所有的警告都当做错误来处理 (在Xcode的编译设置页：Build Settings screen有一个设置项，将警告当做error) ，所以我会在程序运行前处理修复掉所有的警告信息。 Xcode能很好的指出如上的低级错误，留神这些警告信息能达到事半功倍的效果。

## [](#六、内存信息)六、内存信息

继续之前的操作：运行程序，点击按钮，等待崩溃。是呢，不负所望：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/NSLog-crash.png  "调用堆栈信息")

好惊讶，这次是EXC_BAD_ACCESS错误中的另一种。幸运的是，Xcode告诉我们崩溃发生的位置在**buttonTapped:**方法中这一行：

```
NSLog("You tapped on: %s", sender);
```

有时候，这种问题会让我们反应不过来发生了什么，同样不用担心，Xcode提供了帮手，点击黄色的三角形看看有什么错误：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Insert-@.png  "调用堆栈信息")

NSLog()使用的是**Objective-C**类型的**字符串**，并不是**C**类型的**字符串**，所以插入一个**@**来修复：

```
NSLog(@"You tapped on: %s", sender);
```

仔细看发现黄色警告信息并没有消除。因为这行还有其他不知道会不会导致崩溃的问题存在。这同样很有趣，有时候代码运行的好好的，或者说看上去执行的好好的，但是其他不知道什么时候它就崩溃了(当然此类型的崩溃经常发生在客户手机上)。

让我们来看看具体的警告信息:

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Format-string-issue.png  "调用堆栈信息")

**%s**表示的是**C**类型的字符串。**C**类型的字符串实际只是一串连续的以**空字符(NULL character，实际值是0)**结尾的**byte**类型数组。比如，**C**类型的字符串“Crash!”实际在内存中的存储如下：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/C-string.png  "调用堆栈信息")

如果你的方法或函数中用到了**C**类型的字符串，那必须先确认字符串是以**0**结尾的，否则函数处理时没办法识别出字符串的结尾。

现在，当你在**NSLog()**格式的字符串或者**NSString**的**stringWithFormat**的字符串中使用**%s**，参数就会被当做**C**类型的字符串解析。这种情况下，作为参数传入的**“sender”**实际是一个**UIButton**的对象，并不是个**C**类型的字符串。甚至当**“sender”**指向的内存中包含字节0，**NSLog()**将不会崩溃，而是输出类似如下的信息：

```
You tapped on: xËj
```

可以准确的看到这些信息来自何处。再一次运行程序，点击按钮等待崩溃。在左侧的调试区，右击**“sender”**选择**“View Memory of \*sender”**项（确认点击的是带*号的）。

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/View-memory-menu.png  "调用堆栈信息")

Xcode这时会这块内存地址存储的内容，也就是刚刚**NSLog()**输出的信息。

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Memory-contents.png  "调用堆栈信息")

然而并不能保证这块内存中一定有NULL字节，所以EXC_BAD_ACCESS错误并不是能轻易报出的。如果一直在模拟器上运行程序，可能跑很长时间都出不来这个错误，因为你自己的测试环境总是你自己最喜欢的状态。这也导致此类型的bug很难浮现。

当然，这种情况发生时Xcode已经给你了类型错误的警告了，所以这种特殊的bug也是很好查找的。但是不论何时，只要使用了**C**类型的字符串或者直接操作了内存，都要特别小心是不是影响了其他地方的内存，导致它们出了问题。

如果应用总是奔溃，那么恭喜你这个bug定位起来实际很容易，但是比较常见的是，应用程序时而崩溃时而不崩溃，这将是问题复现非常困难！这种情况下定位这个bug也将变成史诗级的难题。

修复此处**NSLog()**语句的问题，用下面的方法：

```
NSLog(@"You tapped on: %@", sender);
```

运行程序再一次点击按钮。**NSLog()**做了我们期望它做的事情，但是似乎我们还没有处理好**buttonTapped:**的崩溃。

## [](#七、和调试器做朋友)七、和调试器做朋友

Xcode告诉我们这个最新的crash在这一行：

```[self performSegueWithIdentifier:@"ModalSegue" sender:sender];```

调试窗口没有信息输出。你可以像之前一样一直点击继续运行按钮，或者你也可以在调试器中输入一个命令去获取错误信息。这样做的好处是，程序还是中断在之前的位置。
如果是在模拟器上运行，可以输入如下的**(lldb)**命令：

```(lldb) po $eax```

**LLDB**是Xcode4.3及以上版本中默认调试器。如果使用的是早期的Xcode版本，可以使用**GDB**调试器。他们俩的基本命令一致，所以即便你的Xcode编译命令前面的标记是**(gdb)**而不是上面的**(lldb)**，也一样可以继续（顺便补充一下，you can switch between debuggers in the Scheme editor in Xcode, under the Run action. And you can access the Scheme editor by Alt-tapping the Run icon at the top left corner of your Xcode window.）。**po**命令代表**打印对象（print object）**。参数**$eax**指向一个CPU寄存器。出现异常的情况下，这个寄存器中的数据将包含一个指向**NSException**对象的指针。注意：**$eax**仅在模拟器环境下有效，如果你使用真机调试，那么要访问的寄存器是**$r0**。

例如，这样输入：

```(lldb) po [$eax class]```

将看到这样的信息:

```(id) $2 = 0x01446e84 NSException```

数字并不重要，但是明显你正在处理的**NSException**对象是存储在这里的。

你可以通过这个对象调用任何**NSException**的方法。比如：

```(lldb) po [$eax name]```

这行命令将输出该异常的名称，比如这里是**NSInvalidArgumentException**，另外输出如下命令：

```(lldb) po [$eax reason]```

将告诉我们异常的原因:

```
(unsigned int) $4 = 114784400 Receiver (<MainViewController: 0x6b60620>) has no
segue with identifier 'ModalSegue'
```

>**注**: 调用**“po $eax”**的时候, 通常也会调用到对象的**“description”**方法并且输出, 这种情况下一般就已经给出了错误消息。

正好就解释了刚刚发生了什么：你的本意是执行一个名叫**“ModalSegue”**的**segue**，但是显然**MainViewController**中不存在这个东东。

**storyboard**显示一个**segue**使用的是模态方式，但是你忘记设置它的标识，这是个典型的错误：

![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/Segue-identifier.png  "调用堆栈信息")

修改**segue**的标识为**“ModalSegue”**。再一次运行程序，点击按钮，等待crash。这次不再崩溃了！但是呢，这里遗留了我们下一篇要讨论的问题：显示的tableview不应该为空！

## [](#八、相关篇章)八、相关篇章

所以这个空白的tableview到底是关于什么的? 这是本文的一个悬念，下一篇中会详细解释，同时也会解决一些很有趣的在你编程生涯中曾经遇到过bug。当然通过第二部分学习，你还可以更加充实自己的调试技能，比如**NSLog()**语句，**断点**以及**僵尸对象（Zombie Objects）**。

当所有的这些做完讲完，我承诺程序一定会按照我们期待的那样运行！最重要的是，你已经掌握了技能当你的程序出现这些令人挫败的问题时，你也必将能妥善处理它们。

有任何意见和建议请至论坛找我！


![调用堆栈信息](https://pingju020.github.io/assets/images/my-app-crashed-now-what-part-1/hollemans_100x100.jpg  "调用堆栈信息")这篇文章的作者是iOS Tutorial Team 的成员[Matthijs Hollemans](https://www.raywenderlich.com/about#matthijshollemans),他是一个经验丰富的ios设计开发工作者，他的联系方式：[Google+](https://plus.google.com/111394725745201507398?rel=author) 和 [Twitter](https://twitter.com/mhollemans)





