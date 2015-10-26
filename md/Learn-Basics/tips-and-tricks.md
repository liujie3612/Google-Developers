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
 prompt as is the console.clear() method via the Console API from JavaScript.
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