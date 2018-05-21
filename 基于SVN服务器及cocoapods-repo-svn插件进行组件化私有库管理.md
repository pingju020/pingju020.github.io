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

此处假设我需要在项目**modulization**下做一个**MyExampleKit**的私有库，它的主工程名为**Example**。 
### [](#2.1 创建MyExampleKit私有库)2.1 创建MyExampleKit私有库以下是制作MyExampleKit这个库的步骤。
1. 在SVN根目录modulization下新建MyExampleKit私有库目录。
   在Cornerstone中的REPOSITORIES下找到modulization，展开文件夹，右击选择**New Folder in “modulization”**，给文件夹命名为MyExampleKit。
    
2. svn check out根目录modulization到本地（使用cornerstone或者命令行svn co均可）。
3. 在根目录下新建Example测试工程，并在MyExampleKit目录下新建ExampleKit工程。4. 创建podspec文件：
   打开终端工具，cd进入到MyExampleKit目录下，输入如下命令，生成私有库的ExampleKit.podspec文件。
   
   ``pod spec create ExampleKit``
   
5. 编辑ExampleKit.podspec文件。   输入如下命令，打开该文件：

   ``vim ExampleKit.podspec``
   
   可以看到如下图所示的配置文件
   
   ![ExampleKit.podspec初始文件](https://pingju020.github.io/assets/images/svn_pods_private/exampleKit_podspec_1@2x.png  "ExampleKit.podspec初始文件")
   ![ExampleKit.podspec初始文件](https://pingju020.github.io/assets/images/svn_pods_private/exampleKit_podspec_2@2x.png  "ExampleKit.podspec初始文件")
   ![ExampleKit.podspec初始文件](https://pingju020.github.io/assets/images/svn_pods_private/exampleKit_podspec_3@2x.png  "ExampleKit.podspec初始文件")
   
   其中#开头的行全部为注释行，我们重点要修改的配置项如下有以下几条：
   
   ![ExampleKit.podspec修改版](https://pingju020.github.io/assets/images/svn_pods_private/exampleKit_podspec@2x.png  "ExampleKit.podspec修改版")
   
   需要注意的是在创建ExampleKit工程的时候，不要包含自动化测试框架。
   
6. lint库。保存后在终端工具中输入如下命令：

   ``pod lib lint --allow-warnings``
   
   回车后输出ExampleKit passed validation.说明私有库已经配置好了。至此私有库的创建和配置完成。
  
7. 错误排查。
  
   如果有报错信息可针对具体的提示进行修改，如我上面给出配置实际会有如下错误信息：
   
   ![pod lib lint报错](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint_error@2x.png  "pod lib lint报错")
   
   这时就按照错误提示在命令行中敲如下命令执行：
   
   ``pod lib lint --allow-warnings --verbose``
   
   此时会打印很多编译信息，我们截取部分来看。
   
   首先，刚开头的部分信息如报错信息图1所示：
   
   ![报错信息图1](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint_error_1@2x.png  "报错信息图1")
   
   假设一开始我们并不知道这个意味着什么，所以先不理睬发生了什么，接着往下看。会发现有这么一段输出，这张图命名为报错信息图2：
   
   ![报错信息图2](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint_error_1_end@2x.png  "报错信息图2")
   
   从这里基本已经可以看出是说找不到UIKit这个framework，但是为什么呢。如果此时还没想明白，可以接着往下看报错信息图3：
   
   ![报错信息图3](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint_error_2@2x.png  "报错信息图3")
   
   以及报错信息图4：
   
   ![报错信息图4](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint_error_2_end@2x.png  "报错信息图4")
   
   对照报错信息图1和报错信息图3会发现这两句绿色背景的信息不同：
   
   图1中 `` ExampleKit (0.0.1) - Analyzing on macOS platform.``
   
   图2中 `` ExampleKit (0.0.1) - Analyzing on iOS platform``
   
   所以对应的图3是失败，而图4是成功。那么很明白了，就是我们的ExampleKit在macOS上lint是失败的，在iOS上lint则是成功的。然而，我的初衷是做一个ios的lib，并没有适配macOS，怎么会lint到macOS呢？是不是platform信息设置的不对呢？
   
   此时再去排查上面的的配置信息，就很容易发现果然少了platform的配置。在s.license下面添加：
   
   ``s.platform     = :ios, "5.0"``
   
   添加后再执行 ``pod lib lint --allow-warnings``
   
   此时就通过了：
   
   ![lint通过](https://pingju020.github.io/assets/images/svn_pods_private/pod_lib_lint@2x.png  "lint通过")
   
   至此，一个空白的私有库框架已经完成。
   
   
### [](#2.2 测试私有库)2.2 测试私有库

私有库的测试分如下几步：

1. **build工程**
   
   为了更准确的鉴定库代码的准确性，建议将库代码单独做成一个Framework，里面包含了所有我们需要pod成私有库的代码和资源。
   
   而测试工程则作为一个单独的app工程，以依赖上面的Framework的方式引入需要导入成私有库的代码和资源。
   
   如，我此处刚创建好的Example和ExampleKit的关系如下图所示：
   
   ![工程示意图](https://pingju020.github.io/assets/images/svn_pods_private/project@2x.png  "工程示意图")
   
   这样处理的一个最大的好处就是在将ExampleKit下所有代码和资源导出成私有库前，可以最大限度的将库代码和测试代码剥离，并且处理掉所有代码层面问题。
   
   如果这里编译通过，Example工程可以正常依赖使用ExampleKit.framework,那么就可以进行下一步的测试工作了。
   
2. **pod lib lint**
   
   前面已经提及过pod lib lint，也简单描述过一个lint错误的现象和解决过程。虽然当时我们说很多错误都可以通过``pod lib lint --allow-warnings --verbose``来查看错误信息并解决。
   
   但是，此处因为有代码资源等加进来，或者还涉及到代码路径的问题，经常会有其他问题出现。而这时候的问题多集中在文件资源路径设置不对，依赖的库配置不对等问题上。基本上都是修改podspec文件的设置而解决的。
   
   建议针对pod官方给出的解释，核查处理相关错误。
  
3. **pod install**

   如果上面的lint通过了，那么恭喜，可以进行测试的最后一步了。
   
   先将ExampleKit的所有代码提交到svn。
   
   然后在本地新建个测试工程比如test，并关闭Xcode。
   
   打开终端工具，cd命令进入到test工程所在的目录。
   
   在终端执行命令：
   
   ``pod init``
   
   此时test所在的目录下，多了名为"Podfile"的文本文件。继续在终端执行命令：
   
   ``vim Podfile``
   
   打开Podfile文件后，按下i键，编辑如下内容：
   
   >  #podfile 文件配置
   >
   >workspace 'test.xcworkspace'
   >
   >project 'test.xcodeproj'
   >
   >   #use_frameworks!个别需要用到它，比如reactiveCocoa
   >
   >target 'test' do
   >
   >platform :ios, '7.0'
   >
   >project 'test.xcodeproj'
   >
   >pod 'ExampleKit’, :svn => 'http:XXXX/MyExampleKit’
   >
   >end
   
   编辑好后ESC退出编辑模式，再 按 “：wq” 保存编辑。
   
   此时再到终端执行``pod install``
   
   如果前两步都没什么错误的话，此时应该可以顺利install完成。完成后，Finder进入test目录，双击打开test.xcworkspace，到pod工程下查看导入的ExampleKit私有库。如果需要使用的文件都在，那么便可照pod公共库的方法引用这个私有库的文件。
   
   **注：此处容易出现私有库中缺少文件的情况，一般是该私有库的podspec中配置的文件存放路径不对或者没有导出导致，再仔细核查s.source_files的设置或者s.resources的设置即可。**


### [](#2.3 建立索引库)2.3 建立索引库

操作到上面的pod install通过，私有库可以正常使用话，已经意味这私有库制作完成了。但是注意上面的Podfile的这一行：

>pod 'ExampleKit’, :svn => 'http:XXXX/MyExampleKit’

这里的：svn => 后面实际上是配置的私有库的远程地址。这样操作，对于使用该私有库的开发人员来说，相当不方便。所以，有没有办法能使得我们像使用公共第三方库一样不显式指定私有库地址，但也可以正常使用该库呢？

建立索引库即可。

1. **安装cocoapods-repo-svn插件**
   
   建立基于svn的私有库的索引库等操作需要[cocoapods-repo-svn](https://github.com/dustywusty/cocoapods-repo-svn)插件，使用如下命令安装：
   
   ``sudo gem install cocoapods-repo-svn``
   
2. **关联本地私有库**
   
   在创建本地私有库的代码仓库的时候，我们已经在svn上同时创建了一个空的远程私有索引库，接下来，所以此处我们需要做的就是将该私有库关联到本地。
   
   执行如下命令：
   
   ``pod repo-svn add name url``
   
   其中name为本地索引库的名称，url为远程仓库的地址,建议本地索引库跟远程的私有索引库同名。
   
   关联完成后，我们可以输入下面的命令，查看本地索引：
   
   ``pod repo``
   
   我的本地索引库信息如下图所示：
   
   ![pod repo](https://pingju020.github.io/assets/images/svn_pods_private/pod_repo@2x  "pod repo")
   
   其中master为pod的默认索引库，另外一个就是我们刚刚建立的私有索引库。前往/Users/username/.cocoapods/repos/目录也能看到目录下master目录和我们新建的私有索引库目录。
   
3. **为私有库创建索引**
   
   在终端工具cd进入私有库的specs文件所在目录，并执行如下命令：
   
   ``pod repo-svn push Name xx.podsepc``
   
   Name即私有库的名称。
   
   此时，直接在终端执行 ``pod search Name`` ,就可以找到我们刚刚创建的私有库。
   
4. *完善引用的Podfile文件*
   
   打开Podfile文件后，按下i键，将之前引用ExampleKit库的Podfile修改成如下：
   
   >  #podfile 文件配置
   >
   >workspace 'test.xcworkspace'
   >
   >source 'https://github.com/CocoaPods/Specs.git'
   >
   >plugin 'cocoapods-repo-svn', :sources => [
'http://XXX/ios_modulization_specs'
]
   >
   >project 'test.xcodeproj'
   >
   >   #use_frameworks!个别需要用到它，比如reactiveCocoa
   >
   >target 'test' do
   >
   >platform :ios, '7.0'
   >
   >project 'test.xcodeproj'
   >
   >pod 'ExampleKit’
   >
   >end
   
   有三处改动：
   
   首先，添加了source说明，告诉pod默认索引库需要检索。这是pod版本升级后，使用私有库时如不特别指定检索默认索引库的话，pod只检索指定的私有索引库。
   
   其次，制定了检索的私有库，具体如上面 ``pligin ...``信息所示，告诉pod，需要在这个私有索引库中检索pod的私有库。
   
   最后，我们在pod自己的私有库时，可以和pod第三方开源库一样，直接 ``pod 'ExampleKit’`` 即可。
   
 
 ~本文完~  
   

