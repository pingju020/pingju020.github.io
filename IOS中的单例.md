---
layout: default
---

# [](#IOS中的单例)IOS中的单例

作为IOS开发人员，或多或少都听说过“设计模式”这个名词。设计模式（Design pattern）是一套被反复使用、多数人知晓的、是代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。Cocoa框架内很多提供给我们开发者使用的组件，本身接口的定义或者使用都要借由完成相应的设计模式来实现的，例如单例模式、工厂模式、代理模式等。

本文主要探讨单例模式，以及在IOS中实现单例的方式，以及各自的优缺点。

## [](#一、设计模式的六大原则)一、设计模式的六大原则

**开闭原则（Open Close Principle）**：对扩展开放，对修改关闭

**里氏替换原则（Liskov Substitution Principle）**：子类可以扩展父类的功能，但不能改变父类原有的功能

**依赖倒转原则（Dependence Inversion Principle）**：高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象，核心思想是面向接口编程

**接口隔离原则（Interface Segregation Principle）**：客户端不应该依赖它不需要的接口；一个类对另一个类的依赖应该建立在最小的接口上。

**迪米特法则（最少知道原则）（Demeter Principle）**：一个对象应该对其他对象保持最少的了解

**单一职责原则（ Single responsibility principle ）**：不要存在多于一个导致类变更的原因。通俗的说，即一个类只负责一项职责。

## [](#二、设计模式的分类)二、设计模式的分类

GOF设计模式分类：23种

**创建型设计模式**：对类的实例化过程进行了抽象，能够将软件模块中对象的创建和对象的使用分离。

* 工厂方法模式（Factory Method）
* 抽象工厂模式（Abstract Factory）
* 创建者模式（Builder）
* 原型模式（Prototype）
* 单例模式（Singleton）

**结构型模式**：描述如何将类或者对 象结合在一起形成更大的结构，就像搭积木，可以通过 简单积木的组合形成复杂的、功能更为强大的结构。

* 外观模式/门面模式（Facade门面模式）
* 适配器模式（Adapter）
* 代理模式（Proxy）
* 装饰模式（Decorator）
* 桥梁模式/桥接模式（Bridge）
* 组合模式（Composite）
* 享元模式（Flyweight）

**行为型设计模式**：是对在不同的对象之间划分责任和算法的抽象化。行为型模式不仅仅关注类和对象的结构，而且重点关注它们之间的相互作用

* 模板方法模式（Template Method）
* 观察者模式（Observer）
* 状态模式（State）
* 策略模式（Strategy）
* 职责链模式（Chain of Responsibility）
* 命令模式（Command）
* 访问者模式（Visitor）
* 调停者模式（Mediator）
* 备忘录模式（Memento）
* 迭代器模式（Iterator）
* 解释器模式（Interpreter）

## [](#三、什么是单例模式)三、什么是单例模式

程序在运行的时候，通常都会生成多个实例，例如表示字符串的NSString类的实例与字符串是一对一的关系，所以当有一千个字符串的时候，就会生成1000个实例，

许多时候整个系统只需要拥有一个的全局对象，这样有利于我们协调系统整体的行为。比如在某个服务器程序中，该服务器的配置信息存放在一个文件中，这些配置数据由一个单例对象统一读取，然后服务进程中的其他对象再通过这个单例对象获取这些配置信息。这种方式简化了在复杂环境下的配置管理。(维基百科)。

总结为确保任何情况下都绝对只有一个实例。或者想在程序上表现出“只存在一个实例”这就是单例模式。

cocoa 框架中我们常用的单例有：

* UIApplication(应用程序实例) 
* NSNotificationCenter(消息中心)  
* NSFileManager(文件管理)    
* NSUserDefaults(应用程序设置)     
* NSURLCache(请求缓存)   
* NSHTTPCookieStorage(应用程序cookies池)

## [](#四、IOS中单例的实现)四、IOS中单例的实现
从实例化的方式来说，单例可分为懒加载的和非懒加载的；从线程同步角度划分，单例的实现分为线程安全的和线程不安全的。归类到这些分类下单例的实现方式总共有以下几种：普通懒加载式、线程安全的懒加载式、双重检测懒加载式、普通非懒加载式、静态块式、更完整的单实例。

### [](#4.1 普通懒加载式)4.1 普通懒加载式

“懒加载”实质就是普通意义上的“懒汉式”，是指实例对象在使用时才创建初始化的实例化方式。如下就是普通懒加载实现单例模式的代码示例：

``` IOS

//
//  Singleton.m
//  testSingleton
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton
+(Singleton*)shareInstance{
    if (_instance == nil) {
        _instance = [[self alloc]init];
    }
    return _instance;
}
@end

```

上面这种实现方式初接触单例模式时最容易想到的实现方式，乍看之下也不存在什么问题。但是如果在多线程的情况下，它就没办法达到只有一个实例对象的效果，例如当某个线程1调用shareInstance方法并判断_instance == nil，此时（就在判断为空后[[Singleton alloc]init]之前）另一个线程2也调用shareInstance方法，由于此时线程1还没有实例化出对象，线程2执行shareInstance中_instance 也为空，线程2也会再实例化一个_instance对象出来，所以就没办法达到Singleton类只有一个实例的目的。

所以说普通懒加载式的单例是一种非线程安全的实现单例的方式。所以我们需要线程安全的懒加载式单例，来解决线程安全的问题。

### [](#4.2 线程安全的懒加载式)4.2 线程安全的懒加载式

要实现线程安全的懒加载，可以使用@synchronized，示例代码如下：

``` JAVA

//
//  Singleton.m
//  testSingleton
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton
+(Singleton*)shareInstance{
    
    // 添加同步锁，一次只能一个线程访问。如果有多个线程访问，等待。一个访问结束后下一个。
    @synchronized(self){
        if (_instance == nil) {
            _instance = [[self alloc]init];
        }
    }
    
    return _instance;
}
@end

```

这样加入同步关键字后，也就实现了线程安全访问，因为在任何时候它只能有一个线程访问 shareInstance。但是中同样存在另一个问题：假如我们这个单例对象在程序中使用比较频繁，那么每次使用的时候都需要进行线程同步，而实际上我们仅仅是希望在第一次初始化的时候使用同步而已，所以这个方法还需要进一步优化。

### [](#4.3 双重检测懒加载式)4.3 双重检测懒加载式

由于上面提到的原因，这里我们引入双重检测懒加载式单例实现方法，示例代码如下：

``` JS

//
//  Singleton.m
//  testCopy
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton
+(Singleton*)shareInstance{
    // 示例不存在的时候，再引入同步锁机制，创建初始化示例
    if (_instance == nil){
        // 添加同步锁，一次只能一个线程访问。如果有多个线程访问，等待。一个访问结束后下一个。
        @synchronized(self){
            if (_instance == nil) {
                _instance = [[self alloc]init];
            }
        }
    }
    return _instance;
}
@end

```
这种方法实现的单例已经近乎完美，但是不得不说的是，它也依然存在问题。具体的问题以及解决办法，下面我们讲完非懒加载式后统一说明。

### [](#4.4 普通非懒加载式)4.4 普通非懒加载式

“非懒加载”亦即传统意义上的“饿汉式”，区别于懒加载的延迟加载效果，非懒加载式是指在类加载的时候就实例化了类的单实例对象。
最简单的非懒加载式单例的实现示例代码如下：

``` Objective-C

//
//  Singleton.m
//  testCopy
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton

+(void)load{
    _instance =  [[self alloc]init];
}
+(Singleton*)shareInstance{
    return _instance;
}

@end

```

与懒加载方式相比，这种方式本身就是线程安全的，不需要加锁，执行效率也相对较高。但是也有一些缺点，在类加载时就初始化，会浪费内存，而且在load中的代码初始化，会导致程序启动变缓。

### [](#4.5 静态块式)4.5 静态块式

静态块式是我们平时开发中比较常用的一种方式，示例代码如下：

``` Objective-C

//
//  Singleton.m
//  testCopy
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton

+ (Singleton *)sharedInstance {
    static Singleton *instance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        instance = [[self alloc] init];
    });
    return instance;
}
@end

```

这里使用了GCD的dispatch_once函数，作用正如其名：对于某个任务执行一次，且只执行一次。 dispatch_once函数有两个参数，第一个参数predicate用来保证执行一次，第二个参数是要执行一次的任务block。其在多线程程序中的低负载、高可依赖性、接口简单等特征，正好能很好的解决前面我们讨论过的懒加载方式单例实现的各种弊端。

静态块式的单实例实现也是线程安全的。

### [](#4.6 更完整的单实例)4.6 更完整的单实例

前面我们讨论了五种IOS中单实例的实现方式。但是实际以上的所有的单实例实现方式都不能算作是完整的单实例。举个很简单的例子，如果我们在开发中疏忽并没有按照[Singleton sharedInstance]来获取Singleton的实例，而是直接用[[Singleton alloc]init]的方式，实际还是没办法确保正在的只有一个实例。另外，对该对象进行copy mutableCopy copyWithZone 等操作时，也应该保证是同一份对象。

所以比较完整的单实例代码实际应该入下面实例一样，

``` Objective-C

//
//  Singleton.m
//  testCopy
//
//  Created by pingju020 on 2017/8/17.
//  Copyright © 2017年 pingju020. All rights reserved.
//

#import "Singleton.h"

static Singleton* _instance = nil;
@implementation Singleton

+ (Singleton *)sharedInstance {
    static Singleton *instance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        instance = [[super allocWithZone:nil] init];
    });
    return instance;
}

+ (instancetype)allocWithZone:(struct _NSZone *)zone
{
    return [Singleton sharedInstance];
}

- (id)copy
{
    return self;
}

- (id)mutableCopy
{
    return self;
}

+ (id)copyWithZone:(struct _NSZone *)zone
{
    return self;
}

+ (instancetype)alloc
{
    return [Singleton sharedInstance];
}

```
当然，如果使用的MRC的话，最好再添加如下的代码，才更完整些。

``` Objective-C

//  因为只有一个实例， 一直不释放，所以不增加引用计数。无意义。
- (instancetype)retain
{
    return self;
}

- (oneway void)release
{
    // nothing
}

- (instancetype)autorelease
{
    return self;
}

- (NSUInteger)retainCount
{
    return NSUIntegerMax; // 返回整形最大值。
}


```

## [](#五、永远实现不了的绝对单例)五、永远实现不了的绝对单例

前面我们总共讨论了IOS上六种单例的实现方式。但是这一节，却不得不说一个很让人遗憾的事实，那就是其实我们永远没办法实现绝对的单例。

这是为什么呢？

针对一般的IOS应用，如果使用第五种方式或者更严谨的第六种方式来实现单例，已经可以达到单例的诉求。然而在IOS8以后，操作系统开始支持动态链接库。对于通过动态链接方式加载了动态链接库的应用来说，程序的内存空间不是完整一个，而是因加载的动态库数目决定的不确定个相对独立的块。如果一个库向外提供了访问某个单实例类的实例的方法，此时即使使用哪种方法也没办法使得该类的实例唯一。

目前为止，针对此问题，还没有比较好的解决方案。

所以在使用了动态链接库的应用中，对于单例的使用还是慎重为好。

当然另一方面，目前的App Store还不允许上架使用动态链接库的应用，所以这也算以另一个方式控制了IOS上单例并不绝对单例的问题。




