#### console API 参考

console API 提供一些方法来把应用程序的信息打印到控制台，创建 JavaScript 配置文件以及开启调试会话。

#### console.assert(expression, object)

如果指定的表达式值为 false，控制台会显示一个堆栈跟踪的信息。在下面的例子中，当文档包含的子节点少于 10 个时，控制台会输出断言信息。

```js
var list = document.querySelector('#myList');
console.assert(list.childNodes.length < 10, "List item count is >= 10");
```
![assert-failed-list](../../images/assert-failed-list.png)

#### console.clear()

清空控制台。

======================================================

#### console.count(label)

打印同一处的同一标签 count() 被调用的次数。

下面的例子中，每次 login() 函数被调用时， count() 都会被调用。

```js
function login(user) {
	console.count("Login called");
	// login() code...
}
```
![count](../../images/count.png)

下面的示例中，count() 每次调用时的标签都会改变，其中的每个标签都分别递增：
```js
function login(user) {
   console.count("Login called for user '" +  user + "'");
   // login() code...
}
```
![count-unique](../../images/count-unique.png)

#### console.debug(object [, object, ...])

这个方法等同于 console.log()。

#### console.dir(object)

打印所传递对象的 JavaScript 表现形式，如果传递的是一个 HTML 标签，那么会打印它的 DOM 属性，如下：

```js
console.dir(document.body);
```
![consoledir-body](../../images/consoledir-body.png)

你也可以在 console.log() 中使用对象格式化（%O）来打印一个元素的 JavaScript 属性：

```js
console.log("document body: %O", document.body);
```

![consolelog-object-formatter](../../images/consolelog-object-formatter.png)

对一个对象调用 console.dir() 与上面的调用 console.log() 作用是一样的，它们打印出来的都是对象的 JavaScript 属性。

与 console.log() 相比，console.log() 会直接把对象的 XML 格式化打印出来，跟 Elements 面板显示的一致：

```js
console.log(document.body);
```

![consolelog-body](../../images/consolelog-body.png)

#### console.dirxml(object)

打印所传递对象的 XML 表示，与它在 Elements 面板显示的一致。对于 HTML 元素来说，此方法与 console.log() 方法一致。

```js
var list = document.querySelector("#myList");
console.dirxml();
```

- `%O` 是目录的快捷方式
- `%o` 根据对象类型决定是目录还是 XML 目录（non-dom / dom）

#### console.error(object [, object, ...])

与 console.log() 相似，console.log() 还包括调用方法处的堆栈信息。

```js
function connectToServer() {    
	var errorCode = 1;    
	if (errorCode) {        
		console.error("Error: %s (%i)", "Server is  not responding", 500);    
	}
};
connectToServer();
```

![error-server-not-resp](../../images/error-server-not-resp.png)

#### console.group(object[, object, ...])

启用一个日志组并规定可选标题，调用此方法之后，所有的 console 打印都会出现同一个组内，调用 console.groupEnd() 方法之后关闭此打印组。

```js
console.group("Authenticating user '%s'", user);
console.log("User authenticated");
console.groupEnd();
```
![log-group-simple](../../images/log-group-simple.png)

你也可以嵌套组：

```js
// New group for authentication:
console.group("Authenticating user '%s'", user);
// later...
console.log("User authenticated", user);
// A nested group for authorization:
console.group("Authorizing user '%s'", user);
console.log("User authorized");
console.groupEnd();
console.groupEnd();
```

![nestedgroup-api](../../images/nestedgroup-api.png)

#### console.groupCollapsed(object[, object, ...])

与 `console.group()` 一样，都是创建一个打印组，不同的是 `console.groupCollapsed()` 初始时是收起的，而 console.group() 是打开的。

#### console.groupEnd()

关闭上一个 `console.group()` 或 `console.groupCollapsed()` 创建的打印分组，详见 `console.group()` 与 `console.groupCollapsed()` 的示例。

#### console.info(object [, object, ...])

与 `console.log()` 方法相同。

#### console.log(object [, object, ...])

在 console 面板打印一条信息。传递一个或多个对象，其中每个对象以逗号分隔。传递的第一个参数可以包含格式说明，开头是一个百分号，其后为一个字母，表示要应用的格式。

Devtools 支持一下格式：

- %s：规定格式为字符串
- %d or %i：规定格式为整型
- %f：规定格式为浮点型
- %o：规定格式为展开的 DOM 元素（与 Elements 面板的显示一致）
- %O：规定格式为 JavaScript Object 对象
- %c：根据提供的样式来输出字符串

简单的示例：
```js
console.log("App started");
```
下面的示例使用 string(%s) 和 integer(%d) 格式来规定插入的变量 userName 和 userPoints 的值：
```js
console.log("User %s has %d points", userName, userPoints);
```
![log-format-specifier](../../images/log-format-specifier.png)

对同一个 DOM 元素使用 %o 和 %O：
```js
console.log("%o, %O", document.body, document.body);
```
![log-object-element](../../images/log-object-element.png)

使用 %c 来给打印的文本设置样式：
```js
console.log("%cUser %s has %d points", "color:orange; background:blue; font-size: 16pt", userName, userPoints);
```
![log-format-styling](../../images/log-format-styling.png)

#### console.profile([label])

打开 Chrome Devtools 之后，调用这个函数来创建一个 JavaScript CPU 配置文件，参数为一个可选的 label，完成配置之后调用 console.profileEnd() 方法，每项配置都会加载到 Profiles 栏。

下面的示例中，在一个可能会消耗过多 CPU 资源的函数开始时创建 CPU 配置文件，然后在这个函数退出时结束创建。

```js
function processPixels() {
  console.profile("Processing pixels");
  // later, after processing pixels
  console.profileEnd();
}
```

#### console.profileEnd()

结束当前的 JavaScript CPU 分析会话，如果一个正在运行，那么将会打印报告到 Profiles 面板。

#### console.time(label)

开始一个计时器，参数是一个关联标签，当 console.timeEnd() 使用相同标签调用时，结束此定时器并在 console 面板显示从 console.time() 到 console.timeEnd() 之间的程序运行所花费的时间。时间精确到微秒。
```js
console.time("Array initialize");
var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
    array[i] = new Object();
};
console.timeEnd("Array initialize");
```
![time-duration](../../images/time-duration.png)

#### console.timeStamp([label])

这个方法可以在 Timeline 中增加一条事件的记录。这可以让你的代码生成的时间戳与其他事件相关联，如屏幕布局和绘图，这些都会自动加入到 Timeline 中。

#### console.trace(object)

打印调用此方法处的堆栈信息，包含在 JavaScript 源文件中对应行号的链接。有一个计数器记录 trace() 方法在此处调用的次数，如下图所示：

![console-trace](../../images/console-trace.png)

trace() 也可以传入一个参数：

![console-trace-args](../../images/console-trace-args.png)

#### console.warn(object [, object, ...])

与 console.log() 类似，不过在打印信息时会带上一个黄色的警告框。

```js
console.warn("User limit reached! (%d)", userPoints);
```

![log-warn](../../images/log-warn.png)

#### debugger

全局调试功能，能使浏览器停止程序的执行，并在被调用的行启动调试会话。相当于在 Devtools 的 Sources 面板手动设置断点。

> 注意：debugger 不是 console 对象的方法。

在下面的例子中，当 brightness() 方法被调用时，debugger 也会被打开。

```js
brightness : function() {
    debugger;
    var r = Math.floor(this.red*255);
    var g = Math.floor(this.green*255);
    var b = Math.floor(this.blue*255);
    return (r * 77 + g * 150 + b * 29) >> 8;
}
```

![debugger](../../images/debugger.png)