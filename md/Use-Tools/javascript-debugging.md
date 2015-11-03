## Debugging JavaScript

由于 JavaScript 应用程序的复杂性的增加，开发者需要高效的 JavaScript 调试工具来快速发现问题出现的原因并有效地解决它。 Chrome Devtools 包含很多有用的工具，这些工具可以帮助减少调试 JavaScript 的痛苦。

在这一部分，我们将通过调试 [Google Closure hovercard demo](https://rawgit.com/google/closure-library/master/closure/goog/demos/hovercard.html) 和其他例子来演示如何使用这些工具。

>注意：如果你是一位 web 开发人员，并且希望获取最新的 Devtools 你可以使用[Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)

#### Sources 面板

Sources 面板允许你调试你的 JavaScript 代码，它提供了一个图形界面的 V8 调试器，根据下面的步骤来探索 Sources 面板：

- 打开一个网站，比如[Google Closure hovercard demo](https://rawgit.com/google/closure-library/master/closure/goog/demos/hovercard.html)或[TodoMVC](http://todomvc.com/examples/angularjs/#/) Angular app
- 打开 Devtools 窗口
- 选择 Sources

![Sources](../../../images/javascript-debugging-overview.jpg)

Sources 面板显示当前页面的所有 scripts，面板的图标提供了控制暂停、恢复和跟踪代码，源都是可见的，都是单独的标签，点击![open](../../../images/show-file-navigator.png)可以打开文件浏览，通过文件了浏览可以查看所有打开的脚本。

#### 控制执行

控制执行的按钮都在面板的上面，这些按钮可以让你分部操作你的代码，可用的按钮有：

![continue](../../../images/continue.jpg)Continue：继续执行代码，直到执行到下一个断点

![steppover](../../../images/step_over.jpg)Step over：但不执行代码，可以看见每一行是如何影响变量的，如果你的代码中调用了另一个函数，调试器将不会进入它的代码，而是仍然执行当前函数。

![stepinto](../../../images/step_into.jpg) Step into：与 setp over 相似，然而，在函数调用中点击 Step into 将会导致调试器移动至其执行的第一行。

![stepout](../../../images/step_out.jpg) Step out：当已经进入一个函数时，点击此按钮将会导致函数定义的其余部分将运行并跳转至执行其父级函数的功能。

![tooglebreakpoints](../../../images/disable-breakpoints.png)Toggle breakpoints：切换断点的开启/关闭同时保证它们的启用状态完好。

Sources 面板还有一些快捷键操作：

- Continue：`F8` 或 `Ctrl` + `\` 或 Mac : `Cmd` + `\`
- Step over：`F10` 或 `Ctrl` + `'` 或 Mac : `Cmd` + `'`
- Step into：`F11` 或 `Ctrl` + `;` 或 Mac : `Cmd` + `;`
- Step out：`Shift` + `F11` 或 `Shift` + `Ctrl` + `;` 或 Mac : `Shift` + `Cmd` + `;`
- Next call frame：`Ctrl` + `.`
- Previous call frame：`Ctrl` + `,`

#### 调试断点

一个断点是在脚本中人为的停止或暂停的地方，在 Devtools 中使用断点来调试 JavaScript 代码、更新 DOM 还有网络通信。

#### 增加和删除断点

在 Sources 面板中，打开一个 JavaScript 文件来调试，下面的例子中我们打开了 Angular 的 TodoMVC 中的 todoCtrl.js 文件。

![todoCtrl](../../../images/sources-select-todoCtrl-js.png)

点击行号可以在此行设置断点，带有蓝色标签的行号表示已经在此行设置断点。

![设置断点](../../../images/sources-view-region.jpg)

你可以通过点击行号来设置多个断点，每个设置断点的地方，此行右边会有一个蓝色标签。

断点可以像复选框一样启用和禁用，一个断点被禁用，蓝色的标签也会消失。

点击断点入口可以跳转到源文件中的此断点行号。

![region](../../../images/multiple-breakpoints-region.jpg)

右击蓝色标签会显示一个菜单，此菜单包含以下选项：Continue to Here, Remove Breakpoint, Edit Breakpoint, and Disable Breakpoint。

![options](../../../images/continue-to-here-region.jpg)

选择 Edit Breakpoint 可以设置条件断点，另外，右击行号，选择 Add Conditional Breakpoint，输入可解析为真或假的表达式，当条件为真是断点将暂停执行代码。

![conditional](../../../images/conditional-breakpoint-region.jpg)

当你想分析一段在循环里或事件回调频繁中的代码时，条件断点非常有用。

#### 使用暂停断点

完成设置一个或多个断点之后，返回浏览器窗口页面，下面的例子中在 removeTodo() 方法中增加了一个断点，现在每次在删除 todo 项目的时候都会触发断点的暂停：

![paused](../../../images/breakpoint-paused-app.png)

想要继续执行代码，就需要在 Devtools 窗口单击继续按钮![continue](../../../images/continue.jpg)或使用键盘的 `F8` 快捷键。

虽然脚本暂停了，你仍然可以使用右侧的 Watch、Call Stack、Scope来进行交互。

<strong>Call Stack 面板</strong>

Call Stack 面板显示导致代码暂停的完整的执行路径，给我们提示出造成错误的代码缺陷。

![callstack](../../../images/callstack-region.png)

勾选 Async 可以查看包含异步 JavaScript 回调函数（如：定时器、XHR 事件）的完整路径

![async](../../../images/enable-async-toggle.png)

#### JavaScript 黑盒

当你黑盒一个 JavaScript 源文件时，在你调试代码时就不会跳转进这个源文件，只会调试你自己选择调试的代码。

![blackboxing](../../../images/blackboxing-expanded.png)

你可以在设置面板中设置黑盒脚本，或者在 sources 面板中右键点击一个文件然后选择 Blackbox Script.

![blackbox setting](../../../images/blackboxing-dialog.png)

查看更多黑盒相关，可以访问 goole 官方文档 [Blackboxing JavaScript files](https://developer.chrome.com/devtools/docs/blackboxing)

#### Console drawer

Console drawer 可以让你在当前调试暂停时的 Scope 中做测试，按键盘的 `Esc` 键可以打开或关闭此面板。

![Console drawer](../../../images/console-scope-time-travel.gif)

#### 动态 JavaScript 中的断点 Breakpoints in Dynamic JavaScript




