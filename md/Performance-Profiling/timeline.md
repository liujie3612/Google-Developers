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

* 带有实线的绿条是Chrome绘画调用记录，这发生在主线程旁边的JavaScript，样式计算和布局。合成器线程传递一个绘制调用合成的数据结构。

* 空绿色条是光栅化，合成器催生的工作线程正是处理这些的。

同样是绘画，他们只是代表工作的不同的子任务，如果你有性能问题，你看看你正在改变了什么属性，然后，查看是否有一个合成器，只有这样才能达到同样的目的，[CSS触发器](http://csstriggers.com/)可以帮助识别解决。

####查看帧速率统计

在时间线底部显示的平均帧速率和它的标准偏差代表的是所选的帧范围，如果你的鼠标在帧速率上悬浮，带有帧信息弹框将会显示出来。

* Selected range -- 选定的时间范围内，并且在选定的帧的数字上。
* Minimum Time  -- 选定帧的最慢时间，以及在括号中的相应的帧速率。
* Average Time -- 选定帧的平均时间，以及在括号中的相应的帧速率。
* Maximum Time -- 选定帧的最大时间，以及在括号中的相应的帧速率。
* Standard Deviation -- 变化量所计算出来的平均时间
* Time by category  -- 用在每种类型处理的总的时间，按照类型来进行颜色编码。

![](https://developer.chrome.com/devtools/docs/timeline-images/average.png)

###内存模式

内存视图展现给你由你的应用程序产生的内存使用和在内存中要保持文本数量，DOM节点，事件监听器的计数器情况的视图（那也就是，没有被垃圾回收掉的。）

![](https://developer.chrome.com/devtools/docs/timeline-images/image20.png)

内存模式不能告诉你到底是什么原因造成内存泄漏，但它可以帮助你确定在你的应用程序中哪些事件可能会导致内存泄漏。然后，您可以使用[Heap Profiler](https://developer.chrome.com/devtools/docs/heap-profiling.html)，以确定导致泄漏的特定代码。

###记录

要进行记录，你先开始一个记录会话，与应用程序交互，然后停止录制。它有助于让你知道你想录制的哪种活动，例如，页面加载，一个列表图像中的滚动性能，等等，然后粘贴到那个脚本中。

要进行记录：

1. 打开你想记录的页面。
2. 打开时间线面板，并开始录制通过执行下列操作之一：
    * 单击时间线面板底部的圆形录制按钮。![](https://developer.chrome.com/devtools/images/recording-off.png)
    * 在Mac上按住键盘快捷键Ctrl+E 或者 Cmd+E。
    * 录制过程中录制按钮变为了红色。![](https://developer.chrome.com/devtools/images/recording-on.png)
3. 执行任何必要的用户操作记录所需的行为。
4. 按下现在的红色按钮来停止录制，或者按下快捷键来重复。

###记录一个页面加载

一个常见的任务是从初始的网络请求开始加载页面，在这种情况下，快捷键是有用的，以为这能让你快速的开始一个记录，重新加载页面或者停止记录。

<strong>来记录一个页面加载</strong>

1. 新打开任意一个页面。
2. 打开时间线或者按下 `Cmd`+`E` (Mac)或者 `Ctrl`+`E` (Windows/Linux)来开始录制。
3. 按下`Cmd`+`R`或者`Ctrl`+`R`来快速加载浏览器页面。
4. 当页面已经完成加载时(寻找红色的[event marker](https://developer.chrome.com/devtools/docs/timeline#domcontentloaded-and-load-event-markers))

你的记录应该看起来像下面这样，第一条记录（发送请求）是Chrome的HTTP请求，接下来是为接受HTTP响应一个接受响应记录，接着是一个或多个接收数据记录，一个完成装载记录和解析HTML记录。

![](https://developer.chrome.com/devtools/docs/timeline-images/image06.png)

查看更多每个记录类型的[时间线事件细节引用](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference)

###录制的提示

这里是录制的几个提示：

* **保持记录尽可能的短**，短的记录通常使分析更简单。
* **避免不必要的行为**，试着去避免这些行为，(鼠标点击，网络负载等等)，这些与你想你想要记录和分析的记录是无关的，举个例子，如果你在点击一个“登录”按钮后记录事件，不要再滚动页面，加载图片等等。
* **禁用浏览器缓存**，当记录网络操作时，在开发者工具设置面板中禁用浏览器缓存是一个好的主意。
* **禁用拓展**，Chrome扩展能添加无关的效果到你的时间线记录中。您可以执行下列操作之一：
    * 在[隐身模式](http://support.google.com/chrome/bin/answer.py?hl=en&answer=95464)下打开一个浏览器窗口
    * 创建一个用于测试的新的[用户配置](http://support.google.com/chrome/bin/answer.py?hl=en&answer=142059)文件。
    
##分析时间线记录

本节提供了分析时间线记录的提示。

###关于一个记录查看细节

当你在时间中选择一个时间线,详细信息的窗口显示有关该事件的其他信息。

![](https://developer.chrome.com/devtools/docs/timeline-images/frames_mode_event_selected.png)

某些细节存在于所有类型的事件中，例如持续和CPU时间，而有些只适用于某些事件类型。每种记录包含的细节信息，请查看[Timeline event reference](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference).

当你选择一个画图记录，开发者工具高亮了蓝色半透明区域，如下图所示画面。

![](https://developer.chrome.com/devtools/docs/timeline-images/paint-hover.png)

###DOMContentLoaded和Load事件标记

时间线用一条蓝线和一条红线来诠释每个记录，相应的，都是由浏览器发出的DOMContentLoaded and load事件，当所有的页面DOM内容已经加载和解析完成的时候，DOMContentLoaded事件就完成了，一旦所有的文档资源（图像和CSS文件，等等）已经被完全的加载load事件就完成了。

![](https://developer.chrome.com/devtools/docs/timeline-images/event_markers.png)

###定位强迫同步布局

布局是Chrome计算页面所有元素位置和大小的过程，通常情况下，Chrome从你的应用程序中在响应CSS和DOM布局执行"lazily"布局。 这使得chrome批量改变布局风格而不是为每个需求反应，然而，一个应用程序能够立即让chrome强制执行布局和通过查询特定的布局依赖元素属性（例如**element.offsetWidth.**）的值来查询。这些所谓的“强迫同步布局”可能是一个很大的性能瓶颈，如果频繁的重复或者执行大的DOM树。

时间线能识别当你的应用程序产生一个强制同步布局和用黄色的标记图标(![](https://developer.chrome.com/devtools/docs/timeline-images/image25.png)),当你选择记录的时候，详细信息窗格中包含一个堆栈跟踪的不合格代码。

![](https://developer.chrome.com/devtools/docs/timeline-images/forced_layout.png)

如果一个记录包含一个[子记录](https://developer.chrome.com/devtools/docs/timeline#about-nested-events)来强迫一个布局，父记录标有一个稍微变暗黄色图标，展开父记录，以确定造成强迫布局的子记录。

查看[强迫同步布局示例](https://developer.chrome.com/devtools/docs/demos/too-much-layout/index.html)检测和修复这类性能问题的一个论证。

###关于嵌套事件

在时间线记录中，在视觉中有时会嵌套另外一个事件，你展开“父级”事件来查看嵌套的“子级”事件，这里有两个为什么时间线会嵌套的原因:

* 同步事件发生在一个事件发生的过程中之前，每个事件在内部生成两个原子事件，一个用于开始，一个用于结束时，，该被转换为一个单一的“连续”事件。在这两个原子事件之间发生的其他任何事件成为了外部事件的子级。

下面的截图显示了一个同步事件嵌套的示例。在这种情况下，，当发现需要加载一些外部资源，chrome正在分析一些HTML（解析HTML事件），请求在Chrome完成解析之前就完成了，所以发送请求事件是作为解析HTML事件的子级。

![](https://developer.chrome.com/devtools/docs/timeline-images/sync_events.png)

####带有嵌套事件的时间线记录的颜色

时间线条的颜色编码如下：

* **第一个,深色部分**的条代表了父级事件和它的*所有同步的子级*花费的时间。

* **接下来，略带苍白的颜色**CPU事件和它的*所有同步的子级*花费的时间。

* **苍白的条**代表了表示从第一异步事件的开始到最后它的异步子级的结束时间。

![](https://developer.chrome.com/devtools/docs/timeline-images/image16.png)

选择一个父级记录将会展现下面的细节面板:

* 文本**事件类型汇总**和可视化的饼图
* 在这个点的记录上，使用**js堆**的大小，然后这个操作对于堆大小的影响
* 与事件相关的**其他细节**。

![](https://developer.chrome.com/devtools/docs/timeline-images/parent_record.png)

###过滤和搜索记录

你可以根据类型过滤显示出的记录(例如只显示加载事件),或仅显示长于或等于1毫秒或15毫秒的记录。你还可以以显示匹配字符串的事件来过滤视图。

![](https://developer.chrome.com/devtools/docs/timeline-images/filters.png)

当看着所有的事件，你可能需要去找一个，但是保持它周边的上下文内容，在这种情况下，你可以不通过过滤找到，按`Ctrl`+ `F`（Window/Linux）或 `Cmd`+ `F`（Mac）,当时间线具有显示那些包含搜索词的焦点。

###在时间线部分放大

为了使分析记录更容易，你可以“放大”时间线概述的一部分，来减少记录视图中的时间刻度。

![](https://developer.chrome.com/devtools/docs/timeline-images/image03.png)

要放大时间线部分，执行下列操作之一：

* 在概览区域，用你的鼠标拖动时间线选择。
* 调整标尺区域的灰色滑块。

下面是与时间线选择的一些建议：

* 用当前的选择通过拖动两个滑条之间的区域“擦洗”记录。

![](https://developer.chrome.com/devtools/docs/timeline-images/image26.png)

* 触摸板的用户：

    * 用两个手指在当前时间线选区左右来回刷。
    * 用两个手指分别地在当前时间线选项上放大或收缩

* 鼠标悬于时间线选择上分别的上下滚动鼠标的滚轮来放大或收缩时间线选择。

###保存和加载记录

你可以保存一个时间线记录作为一个JSON文件，然后稍后在时间线中打开。

<strong>保存一个时间线记录：</strong>

1. 在时间线面板里右击或者`Ctrl`+ 单击（只有在Mac）下，然后选择**Save Timeline Data…**,然后按下 `Ctrl`+`S` 快捷键
2. 选择一个位置来保存文件，然后单击保存。

<strong>打开一个存在的时间线记录文件，然后做以下事情之一</strong>

1. 在时间线面板里右击或者`Ctrl`+ 单击，然后选择**Load Timeline Data...**,或者`Ctrl`+`O`快捷键。 
2. 定位到 JSON 文件，然后点击打开。

![](https://developer.chrome.com/devtools/docs/timeline-images/image14.png)

###用户制作时间线事件

应用程序可以添加他们自己的事件到时间线记录中。你可以使用[console.timeStamp()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimestamplabel) 方法来添加原子事件到记录中，或者[console.time()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimelabel)和[console.timeEnd()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimeendlabel) 方法来标记执行时间代码的范围，例如，在下面的记录中**console.timeStamp() **被用于展示一个“添加结果”事件。查看[使用控制台](https://developer.chrome.com/devtools/docs/console.md#marking-the-timeline)[标记时间线](https://developer.chrome.com/devtools/docs/console.md)以获取更多信。

![](https://developer.chrome.com/devtools/docs/timeline-images/adding-result.png)

###在记录中查看CPU时间

你会在时间线记录上看到浅灰色的条，显示出当CPU很忙的时候，在CPU是活动的时候（如下图所示）在时间线区域上鼠标悬停于CPU条上高亮显示。一个CPU条的长度通常是所有（高亮显示）在时间线面板上事件的总和

* 被检查的其他相同进程运行的其他页面，（例如，两个标签从同一站点打开，这个站点做了在**setTimeout()**调用下做了相同的事）

* 取消仪表活动。

![](https://developer.chrome.com/devtools/docs/timeline-images/image24.png)

##时间线事件引用

本节按照记录的类型的属性列出和描述了在一个记录过程中产生的各个类型的记录。

###常见的事件属性

某些细节存在于所有类型的事件，而有些只适用于特定事件类型。本节列出了常见的不同事件类型的属性。特定的事件类型属性在下面被列出来了：

* Aggregated time
    * 对于[嵌套](https://developer.chrome.com/devtools/docs/timeline#about-nested-events)的事件，每一类活动的事件采取的时间。

* 调用堆栈
    * 对于有[子级](https://developer.chrome.com/devtools/docs/timeline#about-nested-events)的事件，每一类活动的事件采取的时间。
* CPU时间
    * 记录时间所花费的CPU时间。
* 细节
    * 其他关于事件的细节。
* 持续时间（在一个事件戳）
    * 带有子级的事件完成所花费的时间，*时间戳*是事件发生的开始时间，
* 自我时间
    * 事件（不包含子级）花费的时间。
* 使用的堆的大小
   * 记录的时候应用程序用的总的内存大小， 和自从采样堆的增量大小
    
###加载事件
    
 本节列出了属于加载类型及属性的事件。   
    
| 事件          | 描述          |
| ------------ |:-------------:|
| 解析HTML      | Chrome执行它的HTML解析算法。 |
| 完成加载      | 一个网络请求完成      |
| 接收数据      | 收到的请求数据。      |
| 接收响应      | 从请求的初始HTTP响应。|
| 发送请求      | 一个网络请求被发出    |

####加载事件属性

* 资源
    * 请求资源的URL
 
* 预览
    * 请求资源的预览（仅适用于图片）
    
* 请求方法
    * 用于请求（例如GET或POST）HTTP方法。
    
* 状态码
    * HTTP响应代码

* MIME 类型
    * 请求资源的MIME类型
    
* 数据编码长度
    * 按字节表示的请求资源的长度。
    

###脚本事件

本节列出属于该脚本的类别和性质的事件。
    
| 事件          | 描述          |
| ------------ |:-------------:|
| 动画帧执行 | 一个预定的动画帧执行，回调其处理程序调用。 |
| 取消动画帧 | 一个定时动画帧被取消。      |
| GC 事件   | 垃圾回收      |
| DOMContentLoaded | 该[DOMContentLoaded](http://docs.webplatform.org/wiki/dom/events/DOMContentLoaded)是由浏览器的触发。当页面上所有的DOM内容被加载和解析完成的时候该事件被触发|
| 评估脚本   | 一个脚本被评估    | 
| 事件      | 一个JavaScript事件(例如"mousedown"或者"key")    | 
| 函数调用   | 一个顶级的JavaScript函数调用（仅当浏览器的JavaScript引擎出现）    | 
| 安装定时器      | 在 setInterval() 或者 setTimeout()方法下创建定时器   | 
| 动画帧请求    | A requestAnimationFrame() 方法预定调用一个新的帧    | 
| 移除定时器      | 先前创建的定时器被清除   | 
| 时间      | 脚本调用[console.time()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimelabel)    |
| 时间结束      | 脚本调用[console.timeEnd()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimeendlabel)   |
| 定时器触发    | 一个预先设定的setInterval() 或者 setTimeout()被触发了  |
| XHR准备状态变化     | XMLHttpRequest对象的就绪状态发生变化。    |
| XHR加载      | 一个XMLHttpRequest加载完毕。    |

####脚本事件准备

* 定时器ID
    * 定时器ID

* 定时时间
    * 定时器设定的定时时间
    
* 重复
    * 如果定时器重复设定的布尔值
* 函数调用
    * 被调用的函数。

###渲染事件

本节列出属于渲染类别及其属性的事件。

    
| 事件          | 描述          |
| ------------ |:-------------:|
| 布局无效 | DOM改变无效的页面布局 |
| 布局 | 执行一个页面布局      |
| 重新计算风格   | Chrome重新计算样式风格      |
| 滚动 | 嵌套视图的内容被滚动 |


####渲染事件属性

* 布局无效
    * 对布局的记录，由无效布局引起的堆栈跟踪的代码

* 需要布局的节点
    * 对于布局记录，在重新布局之前被标记为需要布局的节点数目。
    * 启动，这些通常为开发者的无效代码，这些节点，加上重新布局的根路径。

* 布局树大小
    * 对于布局的记录，在重新布局的根节点下面的总共节点数目(Chrome开始重新布局的节点)

* 布局范围
    * 可能的值是“部分”（重新布局边界是DOM的一部分）或者"整篇文档"

* 元素影响
    * 为重新计算样式记录，由一个样式重新计算影响的元素数目

* 样式无效
    * 对于重新计算式的记录，提供导致样式无效堆栈跟踪的代码

###绘制事件

本节列出属于绘画范畴及其属性的事件。

| 事件          | 描述          |
| ------------ |:-------------:|
| 复合层 | Chrome的渲染引擎合成图像层 |
| 图像解码 | 一个图像资源被解码     |
| 调整图片大小   | 一个图片是从它的原始尺寸开始调整      |
| 绘制 | 复合层被绘制到显示器的一个区域，悬停在一个绘画记录高亮了被更新的区域 |

####绘画时间属性

* 定位
    * 对于绘画时间，矩形的X和Y轴

* 尺寸 
    * 为绘画事件，该区域的高度和宽度。




