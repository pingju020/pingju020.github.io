---
layout: default
---

# [](#用github搭建自己的技术博客)用github搭建自己的技术博客

随着项目的不断增加，需要维护的文档越来越多。而我们目前的项目仓库在公司svn，文档也是以word形式存储在svn上，每次涉及更新编辑冲突处理起来麻烦不说，团队的成员还很难能保持所有人的文档都更新到最新版本。因此为项目进展带来了非常不必要的麻烦。

因此，我们比对好几个解决方案。综合来说，用一个专有的文档库将文档作为网页存储在远端是一个各方面来说都比较优良的方案。综合GitHub的版本控制能力，所以我最终决定用gitpages来做这个事情。

## [](#一、申请git账号)一、申请git账号
首先，进入[GitHub](https://github.com/),输入账号、邮箱、密码,然后点击注册按钮，见图1。

![图1](https://pingju020.github.io/assets/images/file_about_gitpages/register.png  "图1")

注册完成后,选择Free免费账号完成设置，如图2

![图2](https://pingju020.github.io/assets/images/file_about_gitpages/free.png  "图2")
此时打开你的邮箱，查看发送给你的确认邮件，你需要验证邮箱后,后面生成的个人主页才会被接受和发布.
至此，git账号申请完成。

## [](#二、创建页面仓库)二、创建页面仓库
打开[新建仓库页](https://github.com/new)
输入仓库名称、描述、选择是否public，勾选初始化，点“Create repository”按钮创建页面仓库，如图3
注：**页面仓库的名字一定要和你的账号对应**， 如我的账号是pingju020，则我的页面仓库的名称就应该是pingju020.github.io

![图3](https://pingju020.github.io/assets/images/file_about_gitpages/create.png  "图3")
## [](#三、页面设置)三、页面设置
因为这个仓库只存放页面，所以不需要另外创建分支，master即可。进入设置后Repository name、Features、 Merge button、Temporary inteaction limits这些设置项均可不动使用默认值即可。
在GitHub Pages设置选项中的Theme Chooser下选择Change theme按钮，如图4

![图4](https://pingju020.github.io/assets/images/file_about_gitpages/setting.png  "图4")
进入theme 选择页面，如图5

![图5](https://pingju020.github.io/assets/images/file_about_gitpages/choose.png  "图5")
选好theme后，点击页面上的download .zip,下载jekyll皮肤包，然后选择Select Theme确认选择。
此时在浏览器中直接输入你的pages地址，应该显示的是当前git仓库中的readme文件内容。

## [](#四、配置jekyll设置)四、配置jekyll设置
至此，pages已经设置完成，下来我们要做的是如何让我们自己的文章按照我们的设定显示出来。
首先，将git仓库clone到本地。
然后将我们下载到本地的jekyll皮肤包解压，解压后的文件目录如图6

![图6](https://pingju020.github.io/assets/images/file_about_gitpages/files.png  "图6")
将该目录下所有的文件拷贝到刚clone到本地的仓库目录下，原目录下的_cofig.yml和ReadMe.mk会被覆盖。
用文件编辑器打开_cofig.yml文件，我选择的是Slate theme皮肤，打开文件如下：

``` YAML
#page名称
title: Slate theme    

#page简单描述
description: Slate is a theme for GitHub Pages. 

# 是否显示下载皮肤入口
show_downloads: true 

google_analytics:

#皮肤名称
theme: jekyll-theme-slate 
```
此处，我自己修改了title、description和show_downloads
修改完后保存。

此时如果将所有修改commit并sync到git仓库，再在浏览器中打开私有的page地址的话，应该可以看到一个设置生效成你想要的效果的页面，不过此时显示的内容是仓库中index.mk的内容。当然如果你已经有写好的mk文章，直接拷贝粘贴到index.mk中，覆盖原文，则可以看到您的文章了

## [](#五、写MarkDown博客文件)五、写MarkDown博客文件
配置仓库中的index.mk本身就是一个针对我们的GitHub博客的markdown文件撰写指南。
在此我将其翻译出来提供给大家使用

### [](#5.1 指定样式)5.1 指定样式
首先文件开头必须注明样式，一般直接使用默认就ok，写法如下所示：

>\---
>
>layout: default
>
>\---

### [](#5.2 字体设置)5.2 字体设置

文本可以**加粗**、_斜体_或者~~删除线~~

**加粗** 语法：\*\*待加粗内容**

_斜体_ 语法： \_需要斜体显示的内容_

~~删除线~~ 语法：\~~需要加删除线内容~~

### [](#5.3 文章链接)5.3 文章链接

[链接到其他页面](another-page).

实现这个链接的语法：
\[链接到其他页面](another-page)

其中another-page是存在于文本仓库根目录的另一个markdown文件，全名为 another-page.mk

### [](#5.4 标题)5.4 标题

**一级标题**，如本文的标题，语法如下

>\# \[](#用github搭建自己的技术博客)用github搭建自己的技术博客

注意：#和[]之间需要一个空格键隔开

**二级标题**，如本章节标题，语法如下：

>\## \[](#五、写MarkDown博客文件)五、写MarkDown博客文件

同样需要注意：#和[]之间需要一个空格键隔开

**三级标题**，如本小节标题，语法如下：

>\### \[](#5.4标题)5.4标题

注意#和[]之间需要一个空格键隔开，同时想要其他层级的标题，规则如上，以此类推。


### [](#5.5 引入代码)5.5 引入代码

引入一段js代码：

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

其语法如下：
>\`\`\`js
>
>// Javascript code with syntax highlighting.
>
>var fun = function lang(l) {
>
>  dateformat.i18n = require('./lang/' + l)
>  
>  return true;
>  
>}
>
>\`\`\`

引入一段ruby代码：

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

语法如下

>\`\`\`ruby
>
>\# Ruby code with syntax highlighting
>
>GitHubPages::Dependencies.gems.each do |gem, version\|
>
>  s.add_dependency(gem, "= #{version}")
>  
>end
>
>\`\`\`

其他语言代码也类似。


### [](#5.6 list内容)5.6 list内容

*   无序号的list

语法如下：

> \*   无序号的list

1.  有序list1
2.  有序list2

语法如下：

>1.  有序list1
>2.  有序list2

嵌套列表

- 一级列表
  - 二级列表
  - 二级列表
    - 三级列表
    - 三级列表
- 一级列表
  - 二级列表
  - 二级列表
  - 二级列表
- 一级列表
  - 二级列表
  - 二级列表
- 一级列表

语法如下：

>\- 一级列表
>
>>   \- 二级列表
>>  
>>  \- 二级列表
>>>  
>>>    \- 三级列表
>>>    
>>>    \- 三级列表
>    
>\- 一级列表
>
>>  \- 二级列表
>>  
>>  \- 二级列表
>>  
>>  \- 二级列表
>  
>\- 一级列表
>
>>  \- 二级列表
>>  
>>  \- 二级列表
>  
>\- 一级列表


### [](#5.7 插入表格)5.7 插入表格

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |


语法如下：

>\| head1        | head two          | three |
>
>|:-------------|:------------------|:------|
>
>| ok           | good swedish fish | nice  |
>
>| out of stock | good and plenty   | nice  |
>
>| ok           | good \`oreos`      | hmm   |
>
>| ok           | good \`zoute` drop | yumm  |


### [](#5.8 插入HTML标记)5.8 插入HTML标记
<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

语法分别如下：
>\<dl>
>
>\<dt>Name\</dt>
>
>\<dd>Godzilla\</dd>
>
>\<dt>Born\</dt>
>
>\<dd>1952\</dd>
>
>\<dt>Birthplace\</dt>
>
>\<dd>Japan\</dd>
>
>\<dt>Color\</dt>
>
>\<dd>Green\</dd>
>
>\</dl>


### [](#5.9 插入图片)5.9 插入图片

* 插入小图

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

语法：
>\!\[](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

* 插入大图

![](https://guides.github.com/activities/hello-world/branching.png)

语法：
>\!\[](https://guides.github.com/activities/hello-world/branching.png)


### [](#5.10 水平分隔线)5.10 水平分隔线

下面是一条水平分隔线

* * *

语法：

\* * *

* * *
###~全文完~
