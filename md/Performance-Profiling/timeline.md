#性能分析与时间线

当你的应用程序在运行的时候，时间线面板能够让你记录和分析程序的所有活动。它是在你的应用程序中调查感知性能问题最佳的场所。

##时间线面板概述

时间线有三个主要的部分：一个在上面你的概述，一个记录视图和工具栏。

![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_ui_annotated.png)

* 要想开始和停止一个记录，按下按下Record的切换按钮(详见[Making a recording](https://developer.chrome.com/devtools/docs/timeline#making-a-recording))
* 按清除记录按钮，从时间线上清除记录
* Glue异步事件模式能够让你更加轻易的将它们的事件与异步事件相关联起来。(详见：[About nested events](https://developer.chrome.com/devtools/docs/timeline#about-nested-events))
* 你可以根据它们的类型或者持续时间来在时间线上过滤出显示的记录。(详见[Filtering and searching records](https://developer.chrome.com/devtools/docs/timeline#filtering-and-searching-records))

在一个记录持续的时间中，每件事件的记录都在Records view用“瀑布流”的形式展示出来。记录被分为四个基本组之一：加载，脚本，渲染和绘画。这些记录彩色编码如下：

![](https://developer.chrome.com/devtools/docs/timeline-images/image01.png)

例如，下面的记录是HTML页面加载到Chrome后显示的，第一个记录值（发送请求）是Chrome的 HTTP请求。接下来是接受响应请求(相应的HTTP响应)。一些接受数据记录（实际的页面数据）。然后是最终加载记录。想要获取时间线的完整的事件记录和它们的描述，请看[时间线事件引用](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference)

![](https://developer.chrome.com/devtools/docs/timeline-images/image06.png)

当你的鼠标悬停于时间线记录上的时候，将会弹出一个与事件相关的弹框，例如，下面的截图展示了一个图片资源完成加载后相关的Finish Loading记录的细节。[Timeline event reference](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference)解释了可用于每个记录类型的详细信息。

![](https://developer.chrome.com/devtools/docs/timeline-images/image12.png)

除了详细的记录视图之外，你还能用下面的三种模式来检查记录：

* Events mode 按事件类别显示所有记录的事件
* Frames mode 显示页面的渲染性能。
* Memory mode 随着时间的推移显示你的网页的内存使用情况。

###Events mode

在记录期间所有的事件都是被捕捉的，事件模式提供了这样的一个概览，是按照类型进行组织的。一目了然的，你能看到你的最费时的应用，和是什么类型的任务。在这个视图中，水平横条的长度对应了每个事件完成的时间

![](https://developer.chrome.com/devtools/docs/timeline-images/events_mode.png)

当你从时间视图中选择一个时间范围，(详见[Zooming in on a Timeline section](https://developer.chrome.com/devtools/docs/timeline#zooming-in-on-a-timeline-section)),记录视图被限制为只显示那些视图。

![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_records.png)

###Frames mode

帧模式能够洞悉你应用程序的渲染性能。一个“帧”表示的工作有:浏览器必须要去渲染一个单个帧的内容来展示 - 运行JavaScript，处理事件,更新DOM和改变样式，布局和绘制页面。目标是想让你的app运行在60帧每秒(FPS),这就相当于绝大部分但是不是全部的视频播放的60HZ的刷新速率。因此，你的应用大概有16.6ms(1000ms / 60)来为每一帧来做准备。

横跨在帧视图上的水平线代表了目标在60 FPS和30 FPS的帧速率。一个帧的高度对应了渲染帧的时间，填充在每一帧的颜色表示的是每种任务类型所占的百分比。

渲染帧的时间被显示在记录视图的顶部，如果你的鼠标悬停于展示时间之上，关于帧的额外信息将会出现，包括每种任务所花的时间，CPU时间, 和计算FPS。

![](https://developer.chrome.com/devtools/docs/timeline-images/frames_mode.png)

查看使用帧模式的演示的一个例子[ Timeline demo: Diagnosing and fixing forced synchronous layout ](https://developer.chrome.com/devtools/docs/demos/too-much-layout/)

####关于透明或浅灰色的框

你可能注意到了浅灰色或透明(中空)的帧，这些区域分别表示：

* 不是由开发者工具装载的
* 显示刷新周期之间的空闲时间

下面的记录显示了未被装载的活动和空闲时间。

![](https://developer.chrome.com/devtools/docs/timeline-images/clear-frames.png)

想要知道条状之间的空白区域的更多信息，阅读[Chrome工程师Nat Duca的解释](https://plus.google.com/+NatDuca/posts/BvMgvdnBvaQ?e=-RedirectToSandbox)，描述了如果你正在GPU的瓶颈该怎么去评估。

####关于绿条

绘制包含了一个两步的过程，包括，绘制调用和光栅化。

* Draw calls 这是你想绘画的一系列东西，它来自于应用到你元素的css。最终，有一系列的绘制调用不同于Canvas元素的: moveTo, lineTo, and fillRect，虽然他们在[Skia](https://code.google.com/p/skia/)，Chrome的绘画后端中有不同的名字，但是这是一个类似的概念。

* Rasterization 通过绘制调用的步进，和在缓冲区填写实际的像素，可以上传到GPU中合成。

然后，在这种背景下， 带有绿实线的条和空的绿条的区别在什么地方？

![](https://developer.chrome.com/devtools/docs/timeline-images/hollow-green-bars.png)

* 带有实线的绿条是Chrome绘画调用记录，

