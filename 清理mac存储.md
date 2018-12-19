---
layout: default
---

# [](#清理mac硬盘空间)清理mac硬盘空间

mac电脑，特别是用来做ios开发的电脑，很容易在使用过程中出现存储空间不足的情况。下面就记录下我本人常用的集中清理内存空间的方法。

## [](#一、清空废纸篓)一、清空废纸篓

很多时候，我们删除了本地文件后就不再关注了。但是，这个时候文件只是进入废纸篓，实际还是占用存储空间的。所以建议确认不用的文件时，删除到废纸篓后，果断将其从废纸篓删除。

## [](#二、删除使用频率低的软件)二、删除使用频率低的软件

进入应用软件目录，查看应用软件占用存储空间的大小，对于占空间较大的，且使用频率较低的软件，可以直接删除。

## [](#三、清理本地文件夹)三、清理本地文件夹

本地文件夹分documents和downloads

需要分别进入这两个目录，查看目录下文件，对于已不再需要使用的文件，可以直接删除或者转移到外接存储设备。

## [](#四、清理系统占用)四、清理系统占用

如果你使用xcode 可以做如下操作获得更多的空闲存储空间
1. xcode移除DerivedData
可重新生成；会删除build生成的项目索引、build输出以及日志。
路径：~/Library/Developer/Xcode/DerivedData

2. 删除DeviceSupport
如果你是128G的本子,用xcode开发,空间不足时,
删除DeviceSupport里的吧,留两个自己经常使用的就够了
~/Library/Developer/Xcode/iOS DeviceSupport


3. 清除缓存文件
cd ~/Library/Caches/
rm -rf ~/Library/Caches/*
不用命令行方式
点击前往文件夹,输入:~/Library/Caches/
就看到文件目录了,删除caches下的文件即可;

4. 删除所有系统日志
可以使用下面的命令删除：
sudo rm -rf /private/var/log/*
也可以在操作6中截图的路径文件夹看到Log文件夹,删除下面的文件就可以;

5. 删除临时文件路径
cd /private/var/tmp/
也可以在操作6中截图的路径文件夹看到tmp文件夹,删除下面的文件就可以;
6. 禁用SafeSleep休眠模式
当升级到OS X 10.9 Mavericks版本之后，显示隐藏文件命令如下：
//显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles Yes && killall Finder
//不显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles No && killall Finder
显示隐藏文件对照截图目录寻找








防止OS X继续创建该文件，所以我们需要下面的命令生成一个无法被替换的空文件
1  touch sleepimage
2  chmod 000 /private/var/vm/sleepimage
如果你想要重新开启SafeSleep功能，只需下面的命令即可
1  sudo pmset -a hibernatemode 3
2  sudo rm /private/var/vm/sleepimage
操作7 :停止TimeMachine本地备份（这个看你个人喜欢）
sudo tmutil disablelocal
操作8 :嗓音文件删除
如果你不适用文字转语音功能，那么你肯定不会使用到OS X内置的嗓音文件。
你可以删除这些文件重新获得硬盘空间。
在终端应用中，使用下面的命令即可，首先定位到文件所在文件夹：
cd /System/Library/Speech/
然后执行删除命令，将所有嗓音文件删除
sudo rm -rf Voices/*
如果你执行了命令，那么你将无法使用系统的文字转语音功能。
操作9 :不建议尝试
通过下面的命令移除缓存代码：
sudo rm -rf /private/var/folders/
但是别清除,出错了你又不知道怎么改好;

作者：ZIM东东
链接：https://www.jianshu.com/p/17d4e96633ea
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

## [](#五、删除ios数据)五、删除ios数据

## [](#六、重启电脑)六、重启电脑

![图1](https://pingju020.github.io/assets/images/svn_pods_private/modules@2x.png  "图1")

我们实际需要两个远程仓库：一个远程的代码库和一个远程的私有组件索引库。

### [](#1.1 远程代码库)1.1 远程代码库