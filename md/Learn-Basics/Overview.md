#Chrome开发者工具概述

Chrome开发者工具（简称开发者工具），是一套嵌入到Google Chrome浏览器集网页制作和调试为一体的工具。开发者工具能让web开发人员深入的进入到浏览器和web应用的内部。使用开发者工具能有效的追踪布局问题，设置JavaScript的断点，和得到代码优化的建议。

开发者工具文档已经迁移了！
获取最新的教程，文档和更新内容，[请前往Chrome DevTools的新网址](https://developers.google.com/web/tools/chrome-devtools/)


##使用开发者工具

要想使用开发者工具，首先在Google Chrome浏览器中打开一个web页面或者一个web应用，下面两个任选其一：

* 选择位于你的浏览器窗口右上角的**Chrome 菜单** ![](https://developer.chrome.com/devtools/images/chrome-menu.png)，然后选择**工具 > 开发者工具**.

* 右击页面上的任意一个元素然后选择**审查元素**

开发者工具的窗口将会在你的Chrome浏览器的底部打开

有几个非常有用的快捷键来打开开发者工具

* 使用`Ctrl`+`Shift`+`I`(或者在Mac电脑上使用`Cmd`+`Opt`+`I`)来打开开发者工具。
* 使用`Ctrl`+`Shift`+`J`(或者在Mac电脑上使用`Cmd`+`Opt`+`J`)来打开开发者工具并且焦点是在Console栏里。
* 使用`Ctrl`+`Shift`+`C`(或者在Mac电脑上使用`Cmd`+`Opt`+`C`)来打开开发者工具并且是在检查元素的模式下，或者用来关闭检查元素的模式如果此模式已经打开的话。

针对于你每天的工作流程，学习[快捷方式](https://developer.chrome.com/devtools/docs/shortcuts)将会节省你的时间。


##开发者工具窗口

开发者工具是被集中在开发者工具窗口的顶部栏里。每个工具栏都会有相应的一个面板来让你工作在一个特定类型的页面或者应用信息里，其中就包括DOM元素，资源和来源。

![](https://developer.chrome.com/devtools/images/devtools-window.png)
开发者工具正在使用拾色器

总体而言，一共有八类主要的工具组来查看开发者工具：

* [Elements](https://developer.chrome.com/devtools/docs/dom-and-styles)
* [Resources](https://developer.chrome.com/devtools/docs/resource-panel)
* [Network](https://developer.chrome.com/devtools/docs/network)
* Sources
* [Timeline](https://developer.chrome.com/devtools/docs/timeline)
* [Profiles](https://developer.chrome.com/devtools/docs/profiles)
* Audits
* [Console](https://developer.chrome.com/devtools/docs/console)

你可以使用`Ctrl`+`[`和`Ctrl`+`]`快捷键来在面板元素之间进行移动。

##检查DOM和样式

元素面板能让你看到DOM树中所有的一切，而且可以让你飞一样的检查和编辑DOM元素。当你需要去确定HTML代码的片段时，你将会经常去访问Elements那栏的面板。举个例子，当你对一个图片有一个什么样的HTML id和你想知道它的value值是什么的时候。

![](https://developer.chrome.com/devtools/images/elements-panel.png)

在DOM中查看标题元素

[查看更多有关检查DOM和样式的信息](https://developer.chrome.com/devtools/docs/dom-and-styles)


##在控制台下工作

JavaScript控制台主要能给开发者提供测试web页面和应用这两项主要的功能，它正是能实现以下两样功能的一个地方：

* 在开发过程中记录诊断信息
* 用于去诊断文档和开发者工具的shell脚本

你可以在Console面板中通过[控制台的API](https://developer.chrome.com/devtools/docs/commandline-api)方法来打印出诊断信息。例如使用console.log()和console.profile()命令。你也可以直接去评估表达式来使用Line API提供的方法。为了选择元素你可以使用[$()](https://developer.chrome.com/devtools/docs/commandline-api#selector)命令或者使用[profile()](https://developer.chrome.com/devtools/docs/commandline-api#profilename)来启动CPU探查。

![](https://developer.chrome.com/devtools/docs/console-files/expression-evaluation.png)

评估在控制台的一些命令

[阅读更多有关Console的信息](https://developer.chrome.com/devtools/docs/console)

##调试JavaScript代码

作为JavaScript应用程序的复杂性增加，开发者需要更加强有力的调试工具来帮助快速的发现问题和有效的去解决。Chrome开发者工具包含了一系列的有用的工具来帮助减少调试js程序过程中的痛苦。

 ![](https://developer.chrome.com/devtools/images/js-debugging.png)

 一个打到console面板里带有条件的断点

 [阅读更多有关如何使用开发者工具去调试JavaScript](https://developer.chrome.com/devtools/docs/javascript-debugging)

 ##提高网络性能

Network面板提供了网络实时请求和下载的更多深入的资源，确定和解决那些比预估所要花更长时间的请求是优化页面一个很好的步骤。

![](https://developer.chrome.com/devtools/images/network-panel.png)

网络请求的内容

[阅读更多如何提高你的网络性能](https://developer.chrome.com/devtools/docs/network)

##审查

审查面板在一个页面加载的时候能够分析出里面的数据，然后在减少页面加载时间和提高响应速度方面给出建议，为了进一步了解，我们建议使用 [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

![](https://developer.chrome.com/devtools/images/audits-panel.png)

审查面板给出的建议

##提高渲染性能

**时间线**面板在加载和使用web应用或页面的时候对于各个部分所消耗的时间给你一个完整的概述，所有的事件，包括解析JavaScript，计算样式，和重绘都被绘制在一个时间线上。

![](https://developer.chrome.com/devtools/images/timeline-panel.png)

一个时间线上所展示的各种事件。

[了解更多关于如何提高渲染性能](https://developer.chrome.com/devtools/docs/timeline)

##JavaScript和CSS性能

**分析**面板为你分析出web应用和页面的执行时间和内存使用情况，这些能帮助你去理解资源到底消耗在哪，这样有助于优化你的代码。提供的分析功能如下：

*  **CPU分析**显示出你的页面JS函数执行的时间
*  **堆分析**显示了你的页面JS对象和相关的DOM节点的内存分配情况
*  **JavaScript分析**显示了你的js所执行的时间

![](https://developer.chrome.com/devtools/images/profiles-panel.png)

一个堆快照的例子

[了解更多关于使用如何提高JavaScript和CSS性能](https://developer.chrome.com/devtools/docs/profiles)

##检查存储

**资源**面板能让你检查出在被检查页面的资源，它能让你与HTML5数据库，本地存储，Cookies，和应用缓存等进行交互.

![](https://developer.chrome.com/devtools/images/resources-panel.png)

[了解更多关于检查存储资源](https://developer.chrome.com/devtools/images/resources-panel.png)


##延伸阅读

在开发者文档中你可能会找到对你有益值得去回顾的其他几个部分，包括：

* [堆分析](https://developer.chrome.com/devtools/docs/heap-profiling)
* [CPU分析](https://developer.chrome.com/devtools/docs/cpu-profiling)
* [移动设备仿真](https://developer.chrome.com/devtools/docs/device-mode)
* [远程调试](https://developer.chrome.com/devtools/docs/remote-debugging)
* [开发者工具视频](https://developer.chrome.com/devtools/docs/videos)

##更多资源

###获取更多资源

你也可以通过[@ChromiumDev](http://twitter.com/ChromiumDev)或者通过[论坛](https://groups.google.com/forum/?fromgroups#!forum/google-chrome-developer-tools)来进行提问

![](https://developer.chrome.com/devtools/images/image13.png)

输入到控制面板上的样式。

确定要查看谷歌开发者页面上的[Google+](https://plus.google.com/+GoogleChromeDevelopers/posts)

![](https://developer.chrome.com/devtools/images/chrome-devs-gplus.png)



###参与我们

提交一个在开发者工具上的bug或者功能的请求，请使用问题追踪的网页 [http://crbug.com](http://crbug.com/),也请提及开发者工具bug的摘要。

![](https://developer.chrome.com/devtools/images/crbug.png)

[crbug.com](http://crbug.com/)网站上的bug报告类别选择。

任何人都可以通过直接[贡献源码](https://developer.chrome.com/devtools/docs/contributing)来帮助开发者工具变的更加的完善。

###调试拓展

期待使用开发者工具来调试Chrome的拓展应用？关注[开发和调试拓展](http://www.youtube.com/watch?v=IP0nMv_NI1s).这里一个[调试](https://developer.chrome.com/extensions/tut_debugging)的教程。










