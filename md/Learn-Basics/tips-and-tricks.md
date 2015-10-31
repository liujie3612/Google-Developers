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

* 当你的应用运行时，记录和分析所有行为的地方。
















