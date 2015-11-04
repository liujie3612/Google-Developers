#提示和技巧

##控制台

###书写多行命令

当在控制台的多行编辑模式下，你可以把文本块当做你正在使用的一个文本编辑器，`Shift`+`Enter`能让你进入到多行编辑模式下。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolemultiline.png)

当你正在写比一行简单的JavaScript代码更复杂的JavaScript代时，这个将特别的有用。一旦你已经输入了文本块内容，在命令行的底部按下`Enter`它将会马上运行。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolerun.png)

对于多行编辑的持续支持，阅读有关[片段](https://developer.chrome.com/devtools/docs/authoring-development-workflow.html#snippets)的内容，可以保存和执行自定义JavaScripts片段的一个功能，这个功能通常已经集成到了“开发者工具”中。

###检查元素模式下的快捷启动方式

`Ctrl`+`Shift`+`C`或者`Cmd`+`Shift`+`C`将会打开检查元素模式下的“开发者工具”(或者切换聚焦到它)，这样你就能立即的检查页面。重复聚焦到页面，在Mac电脑中，通过使用`Cmd`+`Shift`+`C`能实现相同的效果。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_10.png)

[更多：使用控制台](https://developer.chrome.com/devtools/docs/console.md)

###支持console.table命令的支持

这个命令将会把提供的任何数据以表格布局的方式来显示出来:

	console.table([{a:1, b:2, c:3}, {a:"foo", b:false, c:undefined}]);
	console.table([[1,2,3], [2,3,4]]);

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleg1.png)

有一个以字符串组形式的“列”参数选项，下面的例子，我们定义一个包含了很多“人”的family对象，然后我们能选择哪些列我们需要去使用并展示出来的:

<pre><code>function Person(firstName, lastName, age) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.age = age;
}

var family = {};
family.mother = new Person("Susan", "Doyle", 32);
family.father = new Person("John", "Doyle", 33);
family.daughter = new Person("Lily", "Doyle", 5);
family.son = new Person("Mike", "Doyle", 8);

console.table(family, ["firstName", "lastName", "age"]);</code></pre>

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleperson.png)

同时，如果你只需要输出其中的前两栏,用以下的代码:

	console.table(family, ["firstName", "lastName"]);

[更多：查看对于console.table命令的更多支持 | G+](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleperson.png)

###预览控制台对象的日志

对象产生的日志可以使用集成在“开发者工具”中的console.log()来直接预览，而不需要您进一步的工作。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_12.png)

###传递多个参数风格的控制台日志

正如我们已经证明的，你可以通过 %c 添加样式到你的控制台日志中,就像你在Firebug中做的一样，例如：
	
	console.log("%cBlue!", "color: blue;");

还支持指定块的多个样式：

	console.log('%cBlue! %cRed!', 'color: blue;', 'color: red;');

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_13.png)

[更多：“开发者工具”控制台样式化日志 | G+](https://plus.google.com/115133653231679625609/posts/TanDFKEN9Kn)

###清除控制台历史的快捷方式

使用`Ctrl`+`L`或`Cmd`+`L` [快捷键](https://developer.chrome.com/devtools/docs/shortcuts.html)你就可以轻松地清除你打开的控制台历史记录。在shell提示符下一个clear()命令也是可以用的，JavaScript的Console API是[console.clear()](https://developer.chrome.com/devtools/docs/console.md#clearing-the-console-history)方法。

###成为一个键盘忍着

在“开发者工具”中，你仅仅用`?`就能打开通用设置，你能通过?导航到一个快捷键面板看到所有支持的快捷键。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_14.png)

###从控制台访问元素

选择一个元素，然后在控制台中打出$0,它将被使用于脚本，如果你的页面中使用了jQuery,那么你可以使用$（$0）重新选择在页面上的元素。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_15.png)

你也可以右击输出到控制台上的任何元素，然后点击“显示元素面板”来在DOM树中找到它。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_16.png)

###使用XPath表达式来查询DOM树

XPath是一种在文档中选择节点的查询节点，通常会返回一个节点集，字符串，布尔值或者一个数字。你也可以使用XPath表达式从“开发者工具”的JavaScript控制台去查询DOM树。

$x(xpath)命令将允许你去执行一个查询，请看下面如何使用$x('//img')来查询页面中的所有图片的例子。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17.png)


然而，函数还能传进一个可选的第二个参数，换句话说就是 $x(xpath, context)这样的形式，这将能让我们选择一个特定的内容(比如一个iframe)然后针对于XPath来查询它。

<pre><code>var frame = document.getElementsByTagName('iframe')[0].contentWindow.document.body;
$x('//'img, frame);  </code></pre>

该查询能指定iframe来查询图片。

###输出最后的控制台结果

通过使用$_将会让你看到最后的控制台输出结果，我们可以证明使用这个方法与另外一个XPath的例子：

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17a.png)

###使用 console.dir

[console.dir(object)](https://developer.chrome.com/devtools/docs/console-api.md#consoledirobject)命令列举出了一个可拓展的JavaScript对象可以提供的所有属性。下面是列举出document.body作为可拓展对象的所有属性的例子。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_18.png)

###在一个特定的iframe下运行JS控制台

顺着“开发者工具”向下是一个下拉菜单，这个下拉菜单的内容取决于你当前选项卡，当你是在控制台面板下的时候，这个下拉菜单能让你选择控制台可以运行的frame，在下拉菜单中选择你的frame，然后你就会发现你在任何时间都是在正确的文本中。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_19.png)

###当跳转到一个新页面的时候保持控制台不清空

有的时候当你跳转到一个新的页面的时候，你希望保存好你控制台的历史记录，为了做到这样，在控制台中右击，然后选择“保存日志”，当你跳转到一个新的页面的时候，控制台日志历史将不会被清空了。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_20.png)

###使用**console.time()**和**console.timeEnd()**来测定回环时间

[console.time()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimelabel)成为了一个新的测定时间的api，通过使用[console.timeEnd()](https://developer.chrome.com/devtools/docs/console-api.md#consoletimeendlabel)来把两者的时间记录到控制台中，这是一个为基准回环或者不能成为一个功能的代码特别有用的。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_22.png)

###使用**console.profile()**和**console.profileEnd()**来测定性能

随着“开发者工具”打开，调用[console.profile()](https://developer.chrome.com/devtools/docs/console-api.md#consoleprofilelabel)成为一个JavaScript CPU的评定API。标签的配置文件可以任意通过，例如在下面的例子中在控制台中输入console.profile("Processing")，完成一个评测结束，调用[console.profileEnd()](https://developer.chrome.com/devtools/docs/console-api.md#consoleprofileend)

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_22.png)

每次的评测都添加到[评测](https://developer.chrome.com/devtools/docs/profiles.html)结果中去

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_23.png)

它也被添加到console.profiles[]进行检查数组以后：

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_24.png)

想要了解更多控制台的关键技巧，点击[Using The Console](https://developer.chrome.com/devtools/docs/console.md)

* 通过[Console API](https://developer.chrome.com/devtools/docs/console-api.md)提供的方法来诊断信息。例如such as console.log()和console.profile()。
* 由[Command Line API](https://developer.chrome.com/devtools/docs/commandline-api.md)提供的方法，例如$()方法，例如选择一个元素，或者profile()来开始CPU诊断。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_console.png)

##时间线

###框架分析与时间线帧模式

时间线面板提供了加载页面应用的时候时间分配的一个概述，例如在处理DOM事件上消耗了多久的时间，呈现页面布局和在页面上绘制元素。它能让你深入进到三个部分去发现为什么你的应用很慢：事件，框架和时机的内存分配。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_0.png)

默认的情况下，时间线将不会显示任何数据，但是你可以打开你的应用程序通过点击面板底部的圆圈![](https://developer.chrome.com/devtools/images/recording-off.png)，这样来使它来记录会话。使用`Ctrl`+ `E`或`Cmd`+ `E`快捷键也会触发记录。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_1.png)

这个记录按钮将会从灰色变为红色，然后"时间线"将会为你捕捉页面的时间信息。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_2.png)

相框模式能够让你洞察Chrome浏览器的任务已经完成后，生成在屏幕应用程序的单个帧。在这种模式下，竖条阴影对应的是重新计算样式，合成等等。每个竖条的透明区域对应的是空闲时间，至少是闲置在在你页面的部分。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_3.png)

例如，假设你的第一帧需要15ms来执行，下一帧需要30ms，一个常见的情况是，帧被同步成一样的刷新速率，第二帧用了比15ms稍微长一点的时间去渲染，这里，帧3就错过了“真实的”物理针帧而被在下一帧里渲染，因此，在第二帧的长度就被有效的增加了一倍。

如果你的应用程序没有很多的动画在里面，当浏览器在处理输入事件的时候执行一个重复的动作序列的设计框架是很有益处的。当留下足够长的时间来处理一帧这样的事件，将会使你的应用程序更加的敏感，这就意味着更好的用户体验。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_4.png)

当我们的目标为60tfs时，我们有一个最大的16.66ms去做一切事情，这就意味着时间不是很充足，所以要尽可能多的减少你的动画行为是很重要的。

[阅读更多：在“时间线”的帮助下提高性能 | 开发者文档](https://developer.chrome.com/devtools/docs/timeline.md)

###使用警告来强制停止布局事件

在“时间线”面板里，如果你看到的记录上带有一个黄色的三角形图标，这就表示了你的代码正在触发强制同步布局时间。

你最好要避免不必要的布局触发，因为他们可以对你的网页的性能有显著影响。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_5.png)

[更多阅读：时间线报警是一个性能的预示](https://plus.google.com/u/0/115133653231679625609/posts/A7rYqkMMmW8)

###由别人来分享和分析时间线记录

多亏了时间线的导入导出功能这样你能和别的开发者查看和共享时间表记录。使用`Ctrl` + `E`或者 `Cmd` + `E`快捷键来开始和停止录制，然后可以右击来保存数据。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_6.png)

###注释时间线的记录

你的代码能够通过使用[console.timeStamp()](https://developer.chrome.com/devtools/docs/console.md#marking-the-timeline)方法添加注释到你的时间线记录中。这样有助于在你的web应用里使你的代码和正在进行的和活动和浏览器时间关联起来。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_7.png)

[更多阅读：控制台API-标记时间线 | 开发者工具文档](https://developer.chrome.com/devtools/docs/console.md#marking-the-timeline)

你的应用程序能够通过调用console.timeStamp()方法来添加注释到你的时间线面板里，这将会使得你很简单的在你的web应用里使你的代码和正在进行的和活动和浏览器时间关联起来。在下面的记录里，时间线被注释成“添加结果”字符串。查看使用控制台标记在时间线的示例代码。

###FPS计数/ HUD展示

FPS计数器是一个用于可视化帧非常有用的工具，这个可以在“开发者工具”中的“设置”找到，然后选中“显示FPS仪表”

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_8.png)

当被激活时，你将会看到你的页面右上方有个帧数统计的深色的盒子，该计数器能够用于实时诊断在你的页面上是什么导致的一个下落在帧速率上，而不用来回切换与时间线的视图。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_9.png)

[更多阅读：剖析长绘画时间在持续绘画模式下 |  HTML5Rocks ](http://updates.html5rocks.com/2013/02/Profiling-Long-Paint-Times-with-DevTools-Continuous-Painting-Mode)

请记住，只是跟踪FPS计数器可能导致你没有注意到间歇性JANK帧。使用内容时一定要小心，值得一提的是，在桌面上显示的FPS不等于在设备上显示的FPS,应该特别注意那的性能配置。对于使用配置使用“时间线”的更多技巧，请查看[时间线的性能分析](https://developer.chrome.com/devtools/docs/timeline.md)

* 当你的应用运行时，记录和分析所有行为的地方。它是开始查看感知性能最好的地方。
* 洞察帧速率，Chrome计算页面上的所有元素的位置和大小都是一种记录值，这些各种各样的记录值是在布局过程中生成的。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_timeline.png)

##Profiles

###找出JavaScript内存泄露的“3步快照”技术

* 打开“开发者工具”然后切换到Profiles面板里
* 在你的页面里执行一个动作，使内存泄露。
* 点击“Take Snapshot”来执行一个堆快照。
* 重复执行第二步和第三步三次。
* 选择最新的堆快照
* 更换过滤器“所有的对象”到“快照1到快照2的对象”
* 你现在应该能看到一组新的泄露对象，你可以选择其中的一个然后查看是什么导致的内存泄露

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_25.png)
![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_26.png)

[更多阅读：BloatBusters - 在GMail中消除内存](https://docs.google.com/presentation/d/1wUVmf78gG-ra5aOxvTfYdiLkdGaR9OhXRnOlIcEmu2s/pub?start=false&loop=false&delayms=3000#slide=id.g1d65bdf6_0_0)

###理解在堆探查里的节点。

红色的节点是活着的，因为他们是独立于DOM树的，而且有一个是从JavaScript引用的树中的节点(要么是一个局部变量要么是某个属性)。

黄色的节点表示引用一个对象的属性或者数组元素的分离的节点 - 应该有一个从DOM窗口到元素的链属性[(例如 window.foo.bar[2].baz).]

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_27.jpg)

[更多阅读:理解在堆探查中的节点 | G+](https://plus.google.com/u/0/115133653231679625609/posts/hEMupRLRJSF)

###理解在CPU分析器上的时间分配

在CPU分析器上，“（idle）”时间是被标记的，时间花费在浏览器上的是“（program）”

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_28.png)

###添加堆分析的视图

一个常见的问题是，我们被问什么是“在'开发者工具'> 'Profiles' > '堆快照'中比较，支配，遏制，和概要视图中差异 ”，这些视图提供了数据的额外的分析并由“profiler”暴漏如下：

比较视图的方式显示该对象已被正确地被垃圾回收，可帮助您跟踪内存泄漏。通常一次操作记录和比较两个或者更多内存快照，我们的想法是通过检测释放内存时的增量，然后计数，好让你确认内存泄露的原因。

支配视图分析有助你确认那些没有期望被指向对象的引用是否仍然存在和删除/垃圾回收是不是实际在工作。

摘要视图帮助你捕获基于由构造函数名称分组的类型的对象（和他们的内存使用情况），这种观点对于跟踪DOM泄露特别的有好处。

遏制视图提供了一个比较好的对象结构视图，帮助我们分析在全局名称空间中（即window）中引用的对象，来找出哪些是保存它们的，它也能够分析出闭合和让你在一个较低水平下剖析你的对象。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_29.png)

[更多阅读：驯服独角兽：在Chrome浏览器中宽松的JavaScript内存分析 | AddyOsmani.com](http://addyosmani.com/blog/taming-the-unicorn-easing-javascript-memory-profiling-in-devtools/)

想要了解更多的内存分析的技巧，请深入[剖析内存性能](https://developer.chrome.com/devtools/docs/heap-profiling.html):

* 学会使用堆分析来在你的应用程序中发现内存泄露。
* 在视图中洞察可用数据的不同，包括摘要视图，比较视图，遏制视图和支配视图。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_memory.png)

##资源

###DOM修改的调试

右击一个元素，然后选择“Break on Subtree Modifications”：当一个脚本遍历元素的子元素和修改他们的时候，调试器将会自动让你检查出发生了什么。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_30.png)

另外值得一提的是：暂停于行内样式的属性修改将会对DOM动画的调试很有帮助。

###追踪未捕获的异常

从资源面板中，双击暂停执行脚本按钮!()[https://developer.chrome.com/devtools/images/pause-icon.png]将会阻断代码的执行当一个未捕获异常发生的时候，保存调用栈和当前应用的状态 - 紫色的标记作为这样的一个参考。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_32.png)

###使用console.log来做有条件的断点

“开发者工具”支持有条件的断点，我们所知道的有在你想打断点的行号上点击和像正常一样去应用一个断点。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_33.png)

你然后右击行号选择“编辑断点”,然后就会出现一个能书写表达式的区域，这里定义你的条件（也就是说表达式的执行结果将会在这里停止）

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_34.png)

一个普通的表达式可能是x===5的形式，但是使用console.log语句在这个区域也是完全有效的输入。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_35.png)

这个方法的效果很好，我们能够很容易的看到console.log语句被打印到控制台上。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_36.png)

由于console.log没有一个真正的返回值，带有条件的不确定的语句断点将不能导致执行被暂停，你的代码也会继续去执行，这很像一个硬编码console.log语句而不是你必须要直接修改代码。

[更多阅读: 在JavaScript中的断点 | randomthink.net](http://www.google.com/url?q=http%3A%2F%2Fwww.randomthink.net%2Fblog%2F2012%2F11%2Fbreakpoint-actions-in-javascript%2F&sa=D&sntz=1&usg=AFQjCNE9yz1n3H6Boru1bl11nQdiUwmR4w)

###漂亮的打印出JavaScript

开发者工具支持美化压缩后的JavaScript代码，使其变成可读的形式，要想美化：

* 转到“资源”面板里，然后选择脚本列表你所需要的脚本。
* 下面一步，在“开发者工具”下面的窗口的底部按下“Pretty print”按钮![](https://developer.chrome.com/devtools/images/prettyprint-icon.png)(标有大括号)
* 你的代码现在应该是美化的

之前

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_38.png)

以后

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_39.png)

###"最喜爱的"表达式或变量值

在调试会话期间，为了避免你需要一次次的去书写变量名或者表达式或者多次去检验校对，把它加到“Watch Expression”列表中，如果你直接对它们就行了修改，就相当于刷新了这个值，或者就是看着代码运行时监视它们的变化。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_40.png)

###洞悉内部属性

假设你已经定义了一个变量值"s"然后你执行以下操作：

<pre><code>s.substring(1, 4)  // returns 'ell'</code></pre>

你认为's'是一个字符串吗，也不完全是，它也可以是一个字符串对象包装，在监视表达式里尝试一下代码：

<pre><code>"hello"
Object("hello")
</code></pre>

第一个是有规律的字符串的值，第二个是全功能的对象，这样可能导致混乱，这两个值几乎表现一样，但是第二个具有真实的属性，你也可以设置你自己的。

展开属性列表，你会发现，无论它是一个完全不规则的对象：它都有一个内部的属性[[PrimitiveValue]]，无论原始值存储在哪，你都不能从代码中访问这个属性，但是你现在可以在“开发者工具”中调试。

[阅读更多：在“开发者工具”中学习JavaScript的概念 | GitHub](http://www.google.com/url?q=https%3A%2F%2Fgist.github.com%2Fpaulirish%2F4158604&sa=D&sntz=1&usg=AFQjCNEoLc-D7771J2l_GvAj0QkGOuT0QQ)

### 更便捷的调试XHRs

在调试器中打开“XHRs 断点部分”,你能看到指定任何的URL(甚至是一个子集)，当做一个XHR请求的时候，你的代码应该截断了，甚至告诉它打断每一个XHR:

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_41.jpg)\

###检索注册元素的每个处理程序

随着'元素'面板打开，找DOM树中的一个元素然后点击选择节点，注意:你也可以使用控制台的API**getEventListeners(targetNode)**这么做；

紧接着在右侧，单击展开"事件监听器"一节，在那里，你会发现连接到元素的所有事件侦听器的列表。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_42.png)

###逃去观察控制台

当你在"资源"面板里调试的时候，你有时需要同时访问控制台。只要按一下Escape键，控制台面板将显示。

你可以按照预期在控制台里写和执行JavaScript，但是这么做有个更好的是，当你在一个断点处暂停的时候，JS执行的范围将会是只是在当前暂停文本的范围里。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_43.png)


###当在暂停处打断点的时候变得更加的有效

当你在你的脚本处打了一个断点的时候，有一些非常有用的特性在你处置的点上。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_44.png)

你可能已经知道，你可以在断点处使用“继续”，"步过"，“步入”和“步出”，但也有对各个按钮的键盘快捷键，了解他们，它们会让你在你的代码导航更高效。

检测表达式(在侧边栏的右边)将会出现一个眼睛在监视你的表达式，这样你就不必不断切换回控制台（例如：X=== Y）,调用它下面的堆栈显示出在它之上的函数调用系统已经通过了。

在变量范围里，你可以右击任意一个函数，然后使用“Jump to definition”跳到定义函数的脚本行上。

在“元素”面板上右击任意一个节点可以设置“Break on”修改来实现DOM断点。这对于调试时监听器是否被正确的连接到节点和当它们被调用时发生的事是非常有帮助的。

当可以为XMLHttpRequests设置一个断点的时候，XHR断点面板也是非常有用的。通过输入一个你想检查的子URL字符串去指定一个断点。

###在异常处暂停

你可能想在一个异常抛出来之后暂停下一次的JavaScript代码的执行，然后检查它的调用栈，范围变量和你的应用程序的状态。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_45.png)

一个暂停按钮(![](https://developer.chrome.com/devtools/images/pause-gray.png))在脚本面板的顶部让你在不同的异常处理模式之间切换。你可能不希望停留在所有异常(![](https://developer.chrome.com/devtools/images/pause-blue.png)),除非你调试包装在try / catch语句的代码里。

###在所有的脚本里执行文本搜索

如果你想在加载了所有文件的页面里面搜索一个特定的字符串，你可以使用下面的快捷键来打开搜索面板:

* `Ctrl` + `Shift` + `F` (Windows, Linux)
* `Cmd` + `Opt` + `F` (Max OSX)

这种搜索同时支持正则表达式和大小写

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_50.png)

###在“开发者工具”下调试CoffeeScript和Source Maps

Source Maps提供了一个语言无关的方式来映射出编译后的生产代码回到那个在你的开发环境中创作的原始源代码。

当分析为生产环境产生的代码时，通常是压缩过的，这使得定位到原始源代码非常的困难。

在编译的阶段，source maps可以存储这些信息，允许你去调试生产代码，原文件将会返回给你一个精确的位置具体到某一行，这使得你对于阅读和调试生产环境的代码大不一样，无论是在CoffeeScript中或以其它方式 - 只要它具有一个源映射。

想要在Chrome中支持Source Maps:

* 打开Settings - General
* 选择"Enable source maps" 

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_51.png)

接下来：

* 编译你的CoffeeScript来执行JavaScript；coffee -c myexample.coffee
* 安装 [CoffeeScript Redux](https://github.com/michaelficarra/CoffeeScriptRedux)
* 创建一个Source map文件example.js.map，这将会持有映射信息:$ coffee-redux/bin/coffee --source-map -i example.coffee > example.js.map
* 确保生成的JavaScript文件，example.js，具有如下的源映射URL://# sourceMappingURL=example.js.map

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_52.png)

现在当你正在调试CoffeeScript代码，由于这个声明，你就知道你的原源文件在哪了。

然后你就可以利用这个source map，在你使用UglifyJS2优化压缩阶段去引用这个第一个source map(CS to JS)，然后有一个映射到CoffeeScript 的最小的 JavaScript，也没有没有编译的JavaScript输出。这可以让你的产品代码调试直接回到你的源CoffeeScript代码。

[更多阅读： NetTuts - Source Maps 101](http://net.tutsplus.com/tutorials/tools-and-tips/source-maps-101/)

想要在你的创作流程中了解更多的技巧，深入学习 [Authoring And Development Workflow](https://developer.chrome.com/devtools/docs/authoring-development-workflow.html):

* 学习如何优化开发流程以便节省时间，实现共同开发任务，例如查找文件或者功能，将编辑存储到脚本或者样式表中，或简单地重新安排布局，以更好地满足您的需求。
* 学习新的特性，例如代码片段，可以用于保存和执行JavaScripts定制的片段，这个功能也经常内嵌在了“开发者工具”中。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_authoring.png)

##元素

###允许标尺

在 Settings > General > Show rulers 可以显示出一个标尺当你的鼠标悬停在元素面板上或者选择一个元素的时候。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_53.png)

###自动补全CSS属性

开发者工具支持自动完成已经知道的CSS属性和值(包括需要一个前缀的)，这样对于哪些属性可以为当前元素设置是很有帮助的。

建议你在开始准备为一个属性或者值键入一个名称的时候，你也可以点击右边的右箭头通过滚动可用的选项，我们所熟悉的选项列表将会立刻应用到页面的样式表中。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_55.png)

颜色可以被定义在样式窗格中使用其命名(例如：red), HSL, HEX或者RGB值。你可以shift点击去遍历这些如果需要的话。

如果要显示所有支持的值的属性，你可以使用`Ctrl` + `space`去展示一个完整的建议列表。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_56.png)

这个建议列表是针对具体情况而言的，在某些具体情况下， 例如（字体）的数字，命名和支持的前缀也将会被显示出来。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_57.png)

###颜色拾取器

“开发者工具”内嵌了一个颜色拾取器，当你点击任何一个有效颜色旁边的可视方块的时候，它就显示出来了。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/colorpickercanary.png)

你可以用`Shift` + 点击来改变被选择颜色的格式。 

###添加新的样式

在括号中点击任何一个地方，（包括在"element.style"里），允许你添加一个新的CSS属性，将会立即引用。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_60.png)

一旦你已经完成了添加一个属性，你可以使用tab键来设置下一个属性。新的选择器可以被添加通过点击右手边的"Styles"![](https://developer.chrome.com/devtools/images/plus.png)按钮。这将会允许你定义一个选择器，同样的，根据需要来添加新的属性和值。

你也可以编辑任何一个选择器在“styles”面板中通过单击选择器的名字。一旦名字被改变了，之前的属性将会被覆盖。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_62.png)

新的伪类选择器也可以通过类似的方式来添加，通过把它们附加到选择器的名称上，也可以注意到"toggle element states"[https://developer.chrome.com/devtools/images/attributes-icon.png]按钮，通过点击下面的选项能够切换可视化效果。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_64.png)

让我们回到“匹配CSS规则面板”里，点击右侧的链接将会跳转到Sources面板里，在将显示完整的样式表，并展示出相应的CSS规则存在的行号。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_65.png)

###在元素面板里拖拽

在元素面板里你可以拖拽一个元素在它父级标签以内来改变它的位置，或者将其移动到一个完全不同的部分。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_66.png)

###强制改变元素状态

想要强制某个元素变为特定的状态么？
* 右键点击一个子元素，然后选择“检查元素”
* 在元素面板中右键单击父级元素，然后选择“Force Element State”
* 您现在可以选择下列四个之一 :active, :hover, :focus 或者 :visited，强制元素成为其中之一的状态。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_67.png)

###编写和调试Sass

```提示：在Chrome中去使用Sass去调试你需要有3.3.0版本的(预发布)Sass预编译器，这也是当前支持生成source map唯一支持的版本。 
```
页面当中包含一个CSS预处理器可能会有一个小问题，因为在开发者工具下进行的css修改通常不会应用到Sass的源文件下，这也就意味着如果你在样式上进行了大量的修改，你需要返回去，用一个额外的编辑器手动去将它们应用到源文件上。

这个将不会再是一个问题在最近Sass开发工作流上，要想支持Sass：

1.检查确保你有开发者工具experiments开启的标志

2.接下来，Settings >  Experiments 开启“Sass stylesheet debugging” （注意，这个功能将会很快的移出experiments ）

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/stylesheetdebugging.png)

3.打开General按钮 > Settings >  检查“Enable source maps” 和 “Auto-reload CSS upon Sass save”.

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/autoreload.png)

你也可以保持timeout值为默认值。这个值取决于Sass预处理器完成一个工作需要的时间，你可能需要去修改自动重载的时间，如果需要的话你也可以手动禁用自动重载页面。

4.导航到你想要调试Sass工程所在的页面("开发者工具得是开着的")

5.紧接着，使用下面的标记去启动Sass编译（指定一个css编译目标）：
    sass --watch --sourcemap sass/styles.scss:styles.css
将会有一个Sass监视器在观察为每个生成css文件创建的source map文件(.map)的改变过程，在你的终端下，你应该能看到类似的东西。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_70.png)

这就证实了Sass调试确实在工作。

6.如果安装是在预期之下完成，你可以去元素面板，你需要注意的第一件事是样式表的文件名展现为了.scss源文件，相应的行号也是在你的源文件上的。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_71.png)

7.点开一个文件名将会直接将跳转到Sources面板下，相应的选择器是高亮显示，你现在可以直接在你的 Sass源文件下修改高亮的语句。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_72.jpg)

8.当你在Sources面板下想去编辑Sass文件的时候，你需要确保的是你知道这个文件保存在你本机的哪个地方，右击编辑器，然后选择"另存为"去覆盖你的源文件，当自动加载是开启的，Sass程序也是在后台运行的，我们所修改的将会在Chrome和"开发者工具"编辑器中马上生效。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_73.png)

想要了解更多的在元素和样式下工作的技巧，请查看 [Editing Styles And The DOM:](https://developer.chrome.com/devtools/docs/dom-and-styles.html)

* 在这里你能学习到如何查看页面的真实底层结构，例如，你可能对页面的图片有个id属性，或者那个属性的值时什么很好奇。

* 发现如何去看到一个DOM树中所有的一切。包括检查和及时编辑DOM元素。你可能会经常访问元素的列表，当你需要去确定HTML的某些片段时。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_elements.png)

##网络

###重演XHR

正如你可能知道，网络面板显示了一系列的你的页面发出的请求，XHRs也包含在其中，你可以通过右击请求，然后选择“Replay XHR”去重演任何的XHR请求（POST或者GET）。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_74.png)

###清除网络缓存或者cookies

右击/ `Ctrl`-点击网络面板的任何地方然后选择Clear Browser Cache/Network Cache

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_75.png)

###记录一个跟踪和输出瀑布流

- 点击“record”捕捉多页跟踪

- 请求输出元数据：右击,"Copy Entry as HAR"

- 输出整个瀑布流：右击，"Copy All as HAR"

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_76.png)

[更多阅读: Wait, DevTools Could Do That? | Igvita.com](http://www.google.com/url?q=http%3A%2F%2Fwww.igvita.com%2Fslides%2F2012%2Fdevtools-tips-and-tricks%2F%231&sa=D&sntz=1&usg=AFQjCNHwM1MWYUas6E9zsZxBARF0igWUow)

###想要了解更多的细节使用宽的网络时间线资源行

通过切换在网络面板的底部的“Use large resource rows”图标，你能看到更多的信息。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_77.png)

比较小的资源行视图

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_78.png)

大的资源行视图

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_79.png)

[在网络面板之外了解更多的技巧信息](https://developer.chrome.com/devtools/docs/tips-and-tricks#tricks-for getting more information out of the network panel)

在网络面板里左击时间线的顶部你能获取更多的信息有关网络请求，你可以在下面几项里进行切换：

* Timeline

* Start Time

* Response Time

* End Time

* Duration

* Latency

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/network-left.png)

查看灰色的文字见解：

* 什么是每个请求的HTTP开销？
* 什么是每个请求的首字节时间(TTFB)？ 
* 在响应时间里哪个是最慢的资源？
* 最慢资源的持续时间是多少？

在网络面板里右击顶部的任何一处，你能够启用和禁用下面3个默认没有显示出的选项。

* Cookies
* Domain
* Set-Cookies

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/network-right.png)

###WebSocket检查

在网络面板里，你能够用窗口底部的过滤器来检查WebSocket消息帧。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/websocketbar.png)

例如，导航到[Echo](http://www.websocket.org/echo.html)示例,从网络面板的底部选择"WebSockets" 过滤器，然后点击 "Connect"按钮。你通过'Send'按钮发送的任何消息都可以用 'Frames'子面板来检查。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/websocketdemo.png)

绿色的反映的是由你的客户端发送的，WebSocket的检查是有用的，因为它既能让你查看WebSocket握手，同时也能个人的WebSocket帧

[更多阅读:Wait, DevTools Could Do That? | Igvita.com](http://www.igvita.com/slides/2012/devtools-tips-and-tricks/#1)

[更多阅读: WebSocket inspection with DevTools | Kaazing](http://blog.kaazing.com/2012/05/09/inspecting-websocket-traffic-with-chrome-developer-tools/)

###从网络面板里找到和过滤出XHR

当在网络面板里检查网络请求的时候，你经常需要根据特定的键盘缩小到一个特定的请求。通过使用`Ctrl` + `F` 或者 `Cmd` + `F`这将会被非常容易做到。 

在搜索框里，键入关键字，您正在搜索的关键字和符合那些要求的文件名/URL，将被高亮显示。在输入框的旁边，你可以使用上下按钮在结果里翻页。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_82.png)

尽管这样已经很有用了，它还能提供更多的帮助，只显示符合你搜索条件的结果到你的网络面板里。点击“Filter”就能做到，我们可以看到下面的结果：

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_83.png)

[更多阅读：Evaluating Network Performance | DevTools Docs](https://developer.chrome.com/devtools/docs/tips-and-tricks#dump-network-stack)

###网络栈内部状态的倾倒

"about:net-internals"页面是一个特殊的URL，把网络栈的内部状态展现为视图，这个数据将会对于调试性能和连接问题非常的有用，它包含了性能的请求，代理的设置，和DNS缓存信息。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_84.png)

另外需要注意的是:about:net-internals/#tests也是一个运行测试的特定URL.

想要更多的关于衡量网络性能的技巧，请查看[Evaluating Network Performance](https://developer.chrome.com/devtools/docs/network.md)

* 在你的应用里每个网络是怎么操作的，包括详细的时间数据，HTTP请求和相应头，cookies, WebSocket数据等。
* 学会查看哪个资源有最慢的首包字节，哪些资源加载的时间最长？谁发起的一个特定的网络请求等。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_network.png)

##设置

###模拟touch事件

触摸是一个输入方式，所以非常难以在桌面行进行测试，因为大多数的桌面不具备触摸输入，不得不在移动端进行测试能拉长你的开发周期，因为每一个变化，你都需要发送到服务器，然后在设备上加载。

这种问题的一个解决办法是在你的开发机器上模拟触摸事件，在Chrome DevTools支持单点[触摸事件](http://www.w3.org/TR/touch-events/)仿真，使之更容易在桌面上进行移动应用程序的调试。

要启用触摸事件仿真的支持：

1. 在开发者工具上打开overrides 
2. 向下滚动，然后选择“Enable touch events”

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_85.png)

我们现在可以在标准桌面下通过Sources面板里的事件侦听断点来用相同的方式来调试触摸事件。

[更多阅读：DevTools Device Mode | DevTools Docs](https://developer.chrome.com/devtools/docs/device-mode.html)

###模拟用户代理字符串和设备视口

用你的设备原型然后解决你想支持的特定的移动设备的问题，这种做法是非常容易的，设备模拟可以让这个事变的更加的简单。

开发者工具支持设备模拟，包括本地的UA和尺寸选择，这样就能让开发者在菜单下选择不同的设备，操作系统下就能调试移动端的浏览器。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_86.png)

现在你可以模拟出特定的设备，比如Galaxy Nexus和iPhone，去测试你的媒体查询。

[更多阅读:DevTools Device Mode | DevTools Docs](https://developer.chrome.com/devtools/docs/device-mode.html)

###模拟地理位置

当在HTML5应用程序地理位置支持的情况下，这个特性对于调试在不同的经度和纬度情况下的输出接受是很有好处的。

开发者工具既支持navigator.geolocation的overriding位置值也支持模拟的位置值，这个在overrides菜单里是不可用的

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_87.png)

首要的地理位置

1. 导航到[Geolocation](http://html5demos.com/geo) demo
2. 允许页面访问你的位置，希望这是准确的
3. 打开overrides菜单
4. 检查“Override Geolocation” 然后键入Lat = 41.4949819 and Lat = -0.1461206

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_88.png)

1.刷新页面，这个示例将会现在用你的地理位置。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_89.png)

1.现在检查 “Emulate position unavailable” 选项。
2.刷新页面，示例将会通知你，找到你的位置失败

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_90.png)

 [更多阅读: DevTools Mobile Emulation | DevTools Docs](https://developer.chrome.com/devtools/docs/device-mode.html)

###为视口调试的Dock-to-right 

当你的屏幕比较小的时候，Dock-to-right也是一个对于你的页面如何展示非常有用的模式

* 通过长按在开发者工具底部的图标![](https://developer.chrome.com/devtools/images/layout-button.png)来支持dock-to-right模式。
* 你现在可以左右拖动窗口的界线来改变视口的宽度，这样来触发媒体查询的断点。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_92.png)

###禁用JavaScript

点击右下角的设置齿轮，然后在Settings -> General启用“Disable JavaScript”.

当“开发者选项”是打开的，这个选项也是选中的状态下，JavaScript会在当前页面中被禁用。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/disablejavascript.png)

你如果需要，你也可以在Chrome里运行“-disable-javascript” 来达到相同的效果。

##通用

###在标签之间快速的切换

`Cmd` + `]` 和 `Cmd` + `[` (或者 `Ctrl` + `]` 和 `Ctrl` + `[`) 快捷键可以让你轻松的在开发者工具的标签中左右移动，使用他们避免你以后再手动选择它们。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_94.png)


###学会一个改进的dock-to-right经验

为Elements和Sources面板改进的水平分割线现在是在Chrome beta版本中，现在任何时间切换dock-to-right模式你都能够体验。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_95.png)

然而，如果你有一个非常宽的屏幕，并希望禁用此功能，简单的在设置面板里取消“Split panels vertically when docked to right”就可以了。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/toolbaricons.png)

[更多阅读：More: 3 Steps To A Better Dock-To-Right Experience | G+](https://plus.google.com/u/0/115133653231679625609/posts/8bfFX8BVzTQ)

###使用'Disable Cache'来做到缓存失效

根据设置里，可以启用‘Disable cache’来使磁盘缓存失效。这个对于开发是很有帮助的，但是开发者工具里必须是可见和打开的。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/disablecache.png)

###检查Shadow DOM

具有影子DOM的元素现在将要在元素标签里消失了

1. 确保‘Show Shadow DOM’ 是打开的。
2. 重启开发者工具

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_98.png)

你现在可以窥视影子DOM。例如，你现在可以拿在HTML5 Rocks的Shadow DOM的[标题](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom-201/)举个例子

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_99.png)

###预览所有可检查的页面

如果你经常使用远程调试，你可能会喜爱“about:inspect”，检查出在Chrome中所有的选项卡/扩展。点击'inspect'去切换到一个页面然后启动开发者工具

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_100.png)

###查看app已经缓存的站点

你可以访问 “about:appcache-internals”来获取已经缓存站点的信息，这可以让你看到哪些网站缓存了，它们最后修改的时间，它们用了多少的空间，你也在这可以移除站点缓存。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_101.png)

###在网络/控制台面板中选择多个过滤器

你可能意识到了，在网络和控制台面板中还有可用的过滤器，让你缩小在不同的标准下的数据展示。

你可能不知道的是，你可以使用快捷键(`Cmd` / `Ctrl` + 点击)来选择一个以上的过滤器，下面你能看到在这两个面板中的例子

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_102.png)

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_103.png)

###清除缓存和执行硬重装

如果你需要一个硬刷新，在开发者工具打开的情况下，点击浏览器的刷新按钮，并放于原处，你应该看到一个允许你清除缓存和执行一个硬重载的下拉菜单，这样时间就节省下来了。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_104.png)

###洞察Chrome的任务管理器

在Chrome的任务管理器可以让你洞察到GPU，CPU和JavaScript的内存使用情况，CSS和Script和其他标签的缓存使用情况，

按照以下步骤来打开任务管理器：

1. 点击Chrome浏览器工具栏上的菜单。
2. 选择工具
3. 选择任务管理器
4. 从那里你可以选择额外的视图，通过右击任何一列深入去了解，也可以右击来结束一个进程。

##额外的工具

###调试IOS app

[PonyDebugger](https://github.com/square/PonyDebugger)是使用你的浏览器上的开发者工具来调试你应用的网络流量和管理文本上下文的客户端图书馆和网关服务的组合。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_105.png)

###JSRunTime:开发者工具对于Grepping JavaScript对象的拓展

![Andrei Kashcha](https://plus.google.com/114708134049085210473)为谷歌开发者工具写了一个非常有用的拓展，能够在内存中检索出JavaScript对象的一个图形然后匹配出它们的值或者名称。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_106.jpg)

更多阅读：JSRuntime - A DevTools Extension For Grepping Objects | G+.(https://plus.google.com/u/0/115133653231679625609/posts/ih85hKCyGve)

##其他资源

除了上面的提示，也有许多优秀的拓展资源，幻灯片，文章和视频，但是却鲜为人知的，我们推荐的有：

- [Chrome DevTools Revolutions 2013 (I/O 2013)](http://www.youtube.com/watch?v=x6qe_kVaBpg)
- [Improving Your 2013 Productivity With The Chrome DevTools](http://www.youtube.com/watch?v=kVSo4buDAEE)
- [The DevTools Cheatsheet](http://anti-code.com/devtools-cheatsheet/)
- [Wait DevTools can do that?](http://www.igvita.com/slides/2012/devtools-tips-and-tricks/#1)
- [Chrome DevTools Evolution (I/O 2012)](http://www.youtube.com/watch?v=3pxf3Ju2row)
- [A Re-introduction to the Chrome DevTools (I/O 2011)](http://paulirish.com/2011/a-re-introduction-to-the-chrome-developer-tools)
- [TextMate-like features in Chrome DevTools](http://www.elijahmanor.com/2012/02/textmate-like-t-t-in-chrome-dev-tools.html)
- [Google Chrome Developer Tools: 12 Tricks to Develop Quicker](http://www.youtube.com/watch?v=nOEw9iiopwI)
- [Become a JavaScript Power User With The DevTools](http://www.youtube.com/watch?v=4mf_yNLlgic)
- [Modern Web Development](http://jtaby.com/2012/04/23/modern-web-development-part-1.html)












