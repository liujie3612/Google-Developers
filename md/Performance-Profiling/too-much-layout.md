# 时间线演示：诊断强迫同步布局

这个示例显示了如何使用时间线来确定一种称为“强迫同步布局”的性能瓶颈。这个示例应用来回的使用[requestAnimationFrame()](http://docs.webplatform.org/wiki/apis/timing/methods/requestAnimationFrame)动画几个图片。执行基于帧动画的[推荐的方法](http://updates.html5rocks.com/2012/05/requestAnimationFrame-API-now-with-sub-millisecond-precision)。但是在动画运行的过程中有相当数量的间断和JANK，我们将使用时间线来诊断一下是怎么回事。

使用帧模式和强迫异步布局的更多信息，请[使用时间线](https://developer.chrome.com/devtools/docs/demos/using-timeline)来[定位强迫同步布局](https://developer.chrome.com/devtools/docs/demos/using-timeline.html#locating_forced_synchronous_layouts)。

##做一个记录

首先你要做一个动画的记录

1. 点击**开始**来开始动画。
2. 此页面上打开时间线面板，帧视图下。
3. 在时间线中点击记录按钮。
4. 之后过了一到两秒钟（10-12帧）停止录制并单击**停止**以停止动画。

##分析记录

观察前几个帧很明显，每个都差不多花费300ms开完成，如果鼠标悬停于其中的一个帧上面，一个关于帧的额外信息的弹框将会显示出来。

![](https://developer.chrome.com/devtools/docs/demos/too-much-layout/images/frame-rate.png)

定位一个动画帧触发记录注意它旁边的黄色警告图标，说明一个强制的同步布局。这个图标稍微变暗表明它的一个子记录包含有问题的代码。而不是这个记录本身。扩大动画帧以查看其子级。

![](https://developer.chrome.com/devtools/docs/demos/too-much-layout/images/recording-1.png)

子级记录显示了一个长的重复的**样式重新计算**和**布局**记录的模式。每个布局记录是样式的重新计算的结果。那也就说，是**requestAnimationFrame()**为页面上的每个图片处理**offsetTop**值的请求的结果。鼠标悬停于布局记录之一上然后点击布局强制性质的sources.js链接。

![](https://developer.chrome.com/devtools/docs/demos/too-much-layout/images/layout-warning-hover.png)

在源面板打开的源文件的第43行的update（）函数,是**requestAnimationCallback()**回调处理程序。该处理器计算图片的左CSS样式属性来确定图片的**offsetTop**值。这迫使浏览器立即执行新的布局，以确保它提供了正确的值。

    // Animation loop
function update(timestamp) {
    for(var m = 0; m < movers.length; m++) {
        movers[m].style.left = ((Math.sin(movers[m].offsetTop + timestamp/1000)+1) * 500) + 'px';
        }
    raf = window.requestAnimationFrame(update);
};

我们知道，迫使每一个动画帧在页面布局速度变慢。现在，我们可以尝试直接在开发者工具中解决这个问题。

##开发者工具中应用修复

现在，我们有一个想法是什么引起的性能问题，我们可以在源面板中直接修改JavaScript文件，并立刻测试我们的修改。

1.在先前打开的源面板中，用下面的代码来替换43行
    
    movers[m].style.left = ((Math.sin(m + timestamp/1000)+1) * 500) + 'px';
    
这个版本用(offsetWidth)来替换图片的左样式属性。

2.用另外一个记录验证

动画是明显比以前更快，更流畅，但来衡量与另外一个记录的差异总是一个很好地做法，它看起来应该像下面的记录。

![](https://developer.chrome.com/devtools/docs/demos/too-much-layout/images/fixed.png)

















