---
layout: default
---

#Objective-C基础知识大集合

网上看到个帖子[《招聘一个靠谱的 iOS》](http://blog.sunnyxx.com/2015/07/04/ios-interview/)，里面给出了一系列看似很基础，实则很考察技术水平的面试题。鉴于第一次作答给出的答案并不完全精确，故而萌生了将基础知识再复习一遍，顺带整理个list出来，作为自己温故基础知识的手册。

一点点来，目前只整理了@property和copy相关的知识，持续更新中.

##一、@property
###1.1 @property的本质
@property = ivar(实例变量)+getter + setter（存取方法）

“属性” (property)作为 Objective-C 的一项特性，主要的作用就在于封装对象中的数据。 Objective-C 对象通常会把其所需要的数据保存为各种实例变量。实例变量一般通过“存取方法”(access method)来访问。其中，“获取方法” (getter)用于读取变量值，而“设置方法” (setter)用于写入变量值。这个概念已经定型，并且经由“属性”这一特性而成为 Objective-C 2.0 的一部分。 而在正规的 Objective-C 编码风格中，存取方法有着严格的命名规范。 正因为有了这种严格的命名规范，所以 Objective-C 这门语言才能根据名称自动创建出存取方法。

也可以说:@property = getter + setter;

###1.2 ivar、getter、setter 是如何生成并添加到这个类中的?
完成属性定义后，编译器会自动编写访问这些属性所需的方法，此过程叫做“自动合成”(autosynthesis)。除了生成方法代码 getter、setter 之外，编译器还要自动向类中添加适当类型的实例变量，并且在属性名前面加下划线，以此作为实例变量的名字。

###1.3 @protocol 和 category 中如何使用 @property
在 protocol 中使用 property 只会生成 setter 和 getter 方法声明,我们使用属性的目的,是希望遵守我协议的对象能实现该属性

category 使用 @property 也是只会生成 setter 和 getter 方法的声明,如果我们真的需要给 category 增加属性的实现,需要借助于运行时的两个函数：

1，objc_setAssociatedObject

2，objc_getAssociatedObject

###1.4 ARC下，不显式指定任何属性关键字时，默认的关键字都有哪些？
 **线程安全的**：atomic、nonatomic
 
 **访问权限的**：readonly、readwrite
 
 **内存管理（ARC）**：assign、strong、weak、copy
 
 **内存管理（MRC）**：assign、retain、release
 
 **指定方法名称 (如何定义set get 方法)**：setter = 、getter =
 
 
 由于将来我们经常需要定义一些方法来操作成员变量,而每个方法都必须有一个有意义的名称,而想名字非常难,所以就有了getter-setter方法 getter-setter方法格式和写法都是固定的,所以只要有getter-setter方法我们就不用煞费心思的去想方法名称了,解决我们起名字难问题 并且getter-setter方法还是程序员之间的一种规范,以后别人只要想给属性赋值立刻就会想到getter-setter方法,这样降低了程序员之间的沟通成本

 **setter方法:** 作用: 设置成员变量的值,给成员变量赋值 格式:
 
 setter方法一定是对象方法 ；一定没有返回值 ；一定以set开头, 并且set后面跟上需要设置的成员变量的名称去掉下划线, 并且首字母大写 ；一定有参数, 参数类型一定和需要设置的成员变量的类型一致, 并且参数名称就是成员变量的名称去掉下划线；setter方法的实现中,一定要给带下划线的成员变量赋值 
 
 **getter方法:** 作用: 获取成员变量的值 格式:
 
 getter方法一定是对象方法 ；一定有返回值, 而且返回值一定和获取的成员变量的类型一致 ；方法名称就是获取的成员变量的名称去掉下划线； 一定没有参数 ；getter方法的实现中,一定要返回带下划线成员变量的值
 
##二、copy

###2.1 用@property声明的NSString（或NSArray，NSDictionary）经常使用copy关键字，为什么？如果改用strong关键字，可能造成什么问题？

重写带copy关键字的setter方法

>-  (void)setName:(NSString *)name {
>
>  _name = [name copy];
>
>}

其一， 因为父类的指针可以指向子类的对象，使用copy的目的是为了让本对象的属性不受外界影响，使用copy无论给我传入一个可变对象还是不可变对象，我本身持有的就是一个不可变的副本。

另外，如果我们使用时strong，那么这个属性就有可能指向一个可变对象，如果这个可变对象在外部修改了，那么会影响该属性，copy特质所表达的所属关系与strong类似，然而设置方法并不保留新值，而是将其拷贝，当属性类型是NSString时，经常用此特质来保护其封装性，因为传递给设置方法的新值有可能指向一个NSMutableString类的实例，这个类是 NSString 的子类，表示一种可修改其值的字符串，此时若是不拷贝字符串，那么设置完属性之后，字符串的值就可能会在对象不知情的情况下遭人更改。所以，这时就要拷贝一份“不可变” (immutable)的字符串，确保对象中的字符串值不会无意间变动。只要实现属性所用的对象是“可变的” (mutable)，就应该在设置新属性值时拷贝一份。

###2.2 深拷贝和浅拷贝

####2.2.1 对非集合类的对象的copy和mutableCopy操作。

在非集合类对象中，对不可变对象进行copy操作，是指针复制，mutableCopy操作时是内容复制。对可变对象进行copy和mutableCopy都是内容复制。

>[immutableObject copy] // 浅复制

>[immutableObject mutableCopy] //深复制

>[mutableObject copy] //深复制

>[mutableObject mutableCopy] //深复制

例如:

>NSMutableString *string = [NSMutableString stringWithString:@"origin"];//copy

>NSString *stringCopy = [string copy];

查看内存，会发现 string、stringCopy 内存地址都不一样，说明此时都是做内容拷贝、深拷贝。即使你进行如下操作：

>[string appendString:@"origion!"]

stringCopy 的值也不会因此改变，但是如果不使用 copy，stringCopy 的值就会被改变。 集合类对象以此类推。 所以，

用 @property 声明 NSString、NSArray、NSDictionary 经常使用 copy 关键字，是因为他们有对应的可变类型：NSMutableString、NSMutableArray、NSMutableDictionary，他们之间可能进行赋值操作，为确保对象中的字符串值不会无意间变动，应该在设置新属性值时拷贝一份。

####2.2.2 对集合类的对象的copy和mutableCopy操作。

集合类对象是指 NSArray、NSDictionary、NSSet ... 之类的对象。

 集合类immutable对象使用 copy 和 mutableCopy 的一个例子：

>NSArray *array = @[@[@"a", @"b"], @[@"c", @"d"]];

>NSArray *copyArray = [array copy];

>NSMutableArray *mCopyArray = [array mutableCopy];

查看内存，可以看到 copyArray 和 array 的地址是一样的，而 mCopyArray 和 array 的地址是不同的。说明 copy 操作进行了指针拷贝，mutableCopy 进行了内容拷贝。但需要强调的是：此处的内容拷贝，仅仅是拷贝 array 这个对象，array 集合内部的元素仍然是指针拷贝。

mutable 对象拷贝的例子：

>NSMutableArray *array = [NSMutableArray arrayWithObjects:[NSMutableString stringWithString:@"a"],@"b",@"c",nil];

>NSArray *copyArray = [array copy];

>NSMutableArray *mCopyArray = [array mutableCopy];

查看内存，copyArray、mCopyArray和 array 的内存地址都不一样，说明 copyArray、mCopyArray 都对 array 进行了内容拷贝。同样，我们可以得出结论

在集合类对象中，对 immutable 对象进行 copy，是指针复制， mutableCopy 是内容复制；对 mutable 对象进行 copy 和 mutableCopy 都是内容复制。但是：集合对象的内容复制仅限于对象本身，对象元素仍然是指针复制。 

>[immutableObject copy] // 浅复制

>[immutableObject mutableCopy] //单层深复制

>[mutableObject copy] //单层深复制

>[mutableObject mutableCopy] //单层深复制
