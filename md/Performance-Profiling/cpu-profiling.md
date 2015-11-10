#解析JavaScript性能

##介绍JavaScript的性能分析

使用google chrome，打开[V8 Benchmark Suite](http://v8.googlecode.com/svn/data/benchmarks/v7/run.html)页面，现在打开chrome开发者工具，导航到性能面板，然后确认“Collect JavaScript CPU Profile”是被选择的，现在点击**Start**按钮或者按下`Cmd` + `E`来开始记录一个JavaScript CPU性能。现在刷新V8基准套件页面。、

当页面已经完成重新加载，基准测试的分数就显示出来了，返回到开发者工具然后点击停止按钮来停止录制或者再次按下`Cmd` + `E` 

![heavy-bottom-up](https://developer.chrome.com/devtools/docs/cpu-profiling-files/heavy-bottom-up.png)

这种**自下而上**的视图列出了对性能影响的功能。这也使您可以检查调用路径的功能。

现在选择自上而下的视图通过点击Bottom Up / Top Down按钮。然后点击**功能**栏左侧的小箭头。**自上而下**的视图显示了调用结构的整体图片，从调用堆栈的顶部开始。

> 提示：您可以点击**百分比**按钮来查看绝对时间。

选择**功能**栏之一的功能，然后点击** Focus selected function**按钮（右边的眼睛图标）

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/focus-selected-function.png)

这个过滤了性能只显示选择的功能和它的调用者。在窗口的右下角点击**重新加载**按钮恢复到原始状态。

在功能栏里选择其中之一功能，然后点击**排除选定功能**的里键(x 图标),根据你选择的功能，你会看到这样的事情：

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/tree-top-down.png)

排除选择功能按钮从配置文件删除选定的功能然后收取它的调用者排除功能的总时间。点击**重新加载**按钮恢复到原始状态。

你可以记录多个配置文件，点击开始分析按钮，重新载入V8基准测试页面，然后点击停止分析按钮

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/sidebar-profile-history.png)

在左边的侧边栏中列出了记录配置文件，右侧的树状视图显示聚集在所选配置文件中的信息。

##使用火焰图

随着时间的推移，火焰图视图提供了一个JavaScript处理的可视化表示，类似于在时间线和网络面板中发现的，在执行完*一个JavaScript和CPU解析*后在细节视图上使用火焰图功能，你能用几种不同的方式去查看解析数据。

###可视化执行路径

通过视觉上分析和理解函数调用的进程你能够更好的理解你的应用程序执行的路径。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart00.png)

###用颜色编码来识别异常值

通过缩小时你能够识别出可以被优化的重复模式，或者更重要的是，你能更加容易发现异常值或意外的执行。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart-outliers.png)

###针对于一个时间尺度(像时间线)可视化的JavaScript探查数据

其他的JavaScript解析记录数据随着时间的推移是汇总的，而火焰图是按时间来记录数据的，这就意味着，当你看到事件发生时，真正的透视JavaScript的执行，例如当你在时间线上看到黄色的大条纹，这是看问题的最佳方式。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart02.png)

> 注意： 横轴是时间，纵轴是调用堆栈，开销较高的功能是宽的，调用堆栈表示Y轴，所以一个高的火焰不一定是值得注意的，注意宽条，而不用管他们的调用堆栈中的位置。

###如何使用火焰图

1. 打开开发者面板，然后去“解析面板”
2. 选择**Record JavaScript CPU profile**然后点击**Start**
3. 当您完成收集数据，单击"Stop"

在解析视图中，在开发者工具底部的选择菜单中选择火焰图可视化。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart03.jpg)

> 注意: 在Profiling面板开发者工具火焰图里启用**High resolution CPU profiling**能提高分析时间精度,启动后，您甚至可以放大到十分之一毫秒来查看。

在面板的顶部是一个显示了整个记录的概要，如下图所示，您可以用鼠标选择它放大概述的特定区域，你也可以点击空白区域然后拖动你的鼠标左右平移，时间刻度详细视图相应的缩小。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart04.png)

在详细信息视图里，一个调用栈表示为一个功能块的栈，对立的一个块被称为低功能块，鼠标悬停于给定的块，显示其函数名和定时数据。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart05.jpg)

* Name - 函数的名称
* Self time -  当前调用函数的完成时间，仅仅包括函数声明的本身，不包括它调用的其他函数。
* Total time - 当前调用函数的完成时间和它调用任何函数的总时间。
* Aggregated self time - 在整个记录中，函数调用的聚合时间，不包括这个函数调用的函数。
* Aggregated total time - 函数所有调用聚合的总时间，包括函数调用的函数。

在火焰图中表示的颜色是相当的随机的，在调用中函数总是被用相同的颜色标记，这可以让你看到执行的图案，然后发现异常值更容易，在时间线中所用的颜色没有相关性。

![](https://developer.chrome.com/devtools/docs/cpu-profiling-files/flamechart06.png)

点击一个功能块打开它包含的JavaScript文件，在函数定义的行。


















































