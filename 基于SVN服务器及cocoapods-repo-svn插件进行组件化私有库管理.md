---
layout: default
---

# [](#基于SVN服务器及cocoapods-repo-svn插件进行组件化私有库管理)基于SVN服务器及cocoapods-repo-svn插件进行组件化私有库管理

对于规模庞大，维护人员较多的代码，组件化无疑是开发维护过程中降低成本的最有效方式。但是鉴于ios对直接使用动态库的支持并不完美（appstore并不允许使用），静态库又容易导致库拷贝重复，带来应用程序体积增大的问题，所以在组件化的过程中，我选择了使用cocoapods创建基于svn的私有库这套方案。

## [](#一、准备工作)一、准备工作

Cocoapods有一个索引容器Spec Repo，所有公开的Pods都记录在索引容器下的master里面。实际上就是一个Git仓库remote端，当你使用了Cocoapods，这个仓库会被clone到本地的~/.cocoapods/repos目录下，进入到这个文件下可以看到master文件夹，就是官方的Spec Repo了。当我们pod search的时候就在master这个文件下面查找需要的库。

如果使用私有库，则当我们设置好本地私有库索引后，查看~/.cocoapods/repos目录，能发现除了公共库的master文件夹，还有我们指定命名的私有索引库出现。

此处以SVN的代码托管为例，完整的pod私有库构成当如下图所示：

![图1](https://pingju020.github.io/assets/images/svn_pods_private/modules@2x.png  "图1")

我们实际需要两个远程仓库：一个远程的代码库和一个远程的私有组件索引库。

### [](#1.1 远程代码库)1.1 远程代码库

对于一般的项目，远程代码库应该都已经存在，且库目录分三大块 trunk，branch和tag。主干代码存放在trunk目录下，branch用来存放分支版本，tag则用来存放标签版本。（**注：**代码库的目录建议如此分配，如果不是这么分配的，也没有关系。）

### [](#1.2 远程索引库)1.2 远程索引库

远程索引库实际上是一个独立于代码库的索引仓库。cocoapods的基本原理中实际也是有个远程索引仓库的，它的地址为：[https://github.com/CocoaPods/Specs.git](https://github.com/CocoaPods/Specs.git)，平时如果只使用公共库的话，不用单独配置公共库地址，cocoapods会自动链接到这个库。
但是做私有库的话，我们就要单独建立个私有的索引库了。

### [](#1.3 安装cocoapods-repo-svn插件)1.3 安装cocoapods-repo-svn插件

cocoapods默认支持的远程仓库为git，而我们使用的远程仓库为SVN，所以我们准备工作阶段先要安装[cocoapods-repo-svn插件](https://github.com/dustywusty/cocoapods-repo-svn)(此处默认已经安装了cocoapods，如未安装，请至[cocoapods官网](https://cocoapods.org/app)，先下载安装cocoapods，不再赘述)。

在终端执行如下命令,安装cocoapods-repo-svn插件：

``gem install cocoapods-repo-svn ``

如果报如下错误：

>ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.
 
 则换成命令：
 
 ``sudo gem install cocoapods-repo-svn ``
 
 
## [](#二、创建私有库)二、创建私有库

安装好cocoapods-repo-svn插件后，准备工作告一段落，可以开始制作私有库了。

此处假设我需要在项目**modulization**下做一个**MyExampleKit**的私有库，它的主工程名为**MainTest**。 
### [](#2.1 制作MyExampleKit私有库)2.1 制作MyExampleKit私有库以下是制作MyExampleKit这个库的步骤。
1. 在SVN根目录modulization下新建MyExampleKit私有库目录。
   在Cornerstone中的REPOSITORIES下找到modulization，展开文件夹，右击选择**New Folder in “modulization”**，给文件夹命名为MyExampleKit。
    
2. svn check out根目录modulization到本地（使用cornerstone或者命令行svn co均可）。
3. 在根目录下新建MainTest测试工程，并在MyExampleKit目录下新建ExampleKit工程。4. 创建podspec文件：
   打开终端工具，cd进入到MyExampleKit目录下，输入如下命令，生成私有库的ExampleKit.podspec文件。
   
   ``pod spec create ExampleKit``
   
5. 编辑ExampleKit.podspec文件。   输入如下命令，打开该文件：

   ``vim ExampleKit.podspec``
   
   可以看到如下图所示的配置文件
   
   ![ExampleKit.podspec初始文件](https://pingju020.github.io/assets/images/svn_pods_private/exampleKit_podspec@2x.png  "ExampleKit.podspec初始文件")
   
 
  其中#开头的行全部为注释行，我们重点要修改的配置项如下有以下几条：
    >s.name         = "ExampleKit"
 >   >s.version      = "0.0.1"
 >   >s.summary      = "演示组件化demo"
 >
 >s.description  = <<-DESC
  >               演示组件化demo组件
  >                   DESC
  >s.homepage     = http://www.baidu.com
 > >s.license      = { :type => "MIT"}
 > >s.source       = {:svn => "XXX/modulization/ExampleKit"}
 > >s.source_files  = "ExampleKit","ExampleKit/**/**"
 > >s.frameworks = "Foundation", "UIKit",
 >  需要注意的是在创建ExampleKi工程的时候，不要包含自动化测试框架。
 6. lint库。保存后在终端工具中输入如下命令：
 
 ``pod lib lint --allow-warnings``
 
  命令回车后输出ExampleKit passed validation.说明私有库已经配置好了。如果有错误提示，也可以针对提示的错误信息做适当修改即可。
  至此，私有库的搭建及配置完成。
  
### [](#2.2 本地测试私有库)2.2 本地测试私有库

  将本地所有的改动提交到配置库后，终端cd命令进入edu_anhui_main工程所在目录vim Podfile并回车，点i进入编辑模式，敲入如下配置：保存输入信息退出vim。再在终端输入pod install回车即可成功加载edu_anhui_contactKi私有库。加载完成后打开edu_anhui_main.xcworkspace文件即可看到如下代码结构： 注：如果库本身还有依赖其他的私有库或者第三方库，除了要在库工程目录添加podfile文件外，更要在.podspec中添加s.dependency "引用的库名称"



### [](#2.1 将远程私有库关联到本地)2.1 将远程私有库关联到本地

将远程索引库关联到本地的方法比较简单，在终端执行如下命令：

``pod repo-svn add name url ``

其中name为本地索引库的名称，url为远程仓库的地址

