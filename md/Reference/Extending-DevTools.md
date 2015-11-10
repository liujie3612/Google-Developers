## 扩展 Devtools

#### 概述

一个 Devtools 扩展添加功能到 Chrome Devtools中，可以添加新的 UI 界面和侧边栏、与被检测页面进行交互、获取网络请求信息等，查看[featured DevTools extensions](https://developer.chrome.com/devtools/docs/extensions-gallery)。Devtools 扩展有一套 Devtools 特定的扩展API：

- [devtools.inspectedWindow](https://developer.chrome.com/extensions/devtools_inspectedWindow)
- [devtools.network](https://developer.chrome.com/extensions/devtools_network)
- [devtools.panels](https://developer.chrome.com/extensions/devtools_panels)

Devtools 扩展与其他扩展一样：可以有一个背景页，内容脚本以及其他项。此外，每一个 Devtools 扩展都有一个 Devtools page，它有权访问 Devtools 的 API。

![devtools-extension](../../images/devtools-extension.png)

#### Devtools page

每次 Devtools 窗口打开都会创建一个 Devtools page的实例。Devtools page 与 Devtools 窗口的存在时间一样。Devtools page 可以访问 Devtools API 和部分扩展 API。具体如下：

- 使用 devtools.panels 的 API 创建并使用面板
- 获取关于调试窗口的信息并使用 devtools.inspectedWindow 的 API 来运行代码
- 使用 devtools.network 的 API 来获取网络请求的信息

#### 创建一个 Devtools 扩展

创建一个 Devtools 页，需要在扩展清单中添加 devtools_page 字段：

```js
{
  "name": ...
  "version": "1.0",
  "minimum_chrome_version": "10.0",
  "devtools_page": "devtools.html",
  ...
}
```
每次 Devtools 窗口打开都会实例化一个扩展清单中的 devtools_page。使用 devtools.panels 的 API 可以添加其他扩展页来当做 Devtools 窗口中的面板或侧边栏。

> devtools_page 字段必须指向一个 html 页面，与 background 字段不同，指定背景页可以直接指定 JavaScript 文件。

chrome.devtools.* API 只适用于 Devtools 窗口中加载的页面。内容脚本和其他扩展页面都没有这些 API，因此，这些 API 只在 Devtools 窗口打开时才能生效。

也有一些 Devtools API 仍处于实验阶段，参阅 [chrome.experimental.* APIs](https://developer.chrome.com/extensions/experimental) 查看实验性 API 以及其使用方法。

#### Devtools UI 元素：面板和侧边栏

除了常用的 UI 元素扩展，如浏览器行为、上下文菜单和弹窗，Devtools 扩展还可以添加 UI 元素到 Devtools 窗口：

- 面板是顶级标签，如 Elements、Sources、Network 面板
- 侧边栏显示的是面板的详细 UI。Elements 面板里的 Styles、Computed Styles、Event Listeners 都是侧边栏窗口。目前只能往 Elements 面板添加侧边栏窗口。（注意：侧边栏窗口的外观可能和下面图片中的不一样，这完全取决于 Chrome 的版本和 Devtools 窗口停靠的位置）

![devtools-extension-ui](../../images/devtools-extension-ui.png)

每个面板都是其自身的 HTML 文件，HTML 文件可能包括其他资源（JavaScript, CSS, images 等）。创建一个基本的面板如下：

```js
chrome.devtools.panels.create("My Panel",
    "MyPanelIcon.png",
    "Panel.html",
    function(panel) {
      // code invoked on panel creation
    }
);
```
在面板或侧边栏中运行的 JavaScript 与 Devtools page 拥有相同的 API。

在 Elements 面板创建一个基本的侧边栏：

```js
chrome.devtools.panels.elements.createSidebarPane("My Sidebar",
    function(sidebar) {
        // sidebar initialization code here
        sidebar.setObject({ some_data: "Some data to show" });
});
```
有几种方法来显示侧边栏中窗口内容：

- HTML 内容，调用 setPage 来让一个 HTML 页面显示到窗口中
- JSON 数据，给 setObject 传递一个 JSON 参数
- JavaScript 表达式，给 setExpression 传递一个表达式参数。Devtools 计算被检测页面的上下文中的表达式并显示该返回值。

对于 setObject 和 setExpression 这两个方法，窗口显示的结果和其在控制台中输出的值是一致的，但是 setExpression 可以显示 DOM 元素和任意 JavaScript 对象，而 setObject 方法只支持 JSON 对象。

#### 扩展组件之间的通信

下面的部分描述了 Devtools 扩展的不同组件之间的通信的一些场景。

<strong>注入一段内容脚本</strong>

Devtools page 不能直接调用 tabs.executeScript。想要从 Devtools page 注入内容脚本，必须使用 inspectedWindow.tabId 属性来更改调试窗口选项卡的 ID以及发送消息到 background page。调用 tabs.executeScript 来从 background page 注入脚本。

下面的代码片段显示了如何使用 executeScript 来注入内容脚本。

```js
// DevTools page -- devtools.js
// Create a connection to the background page
var backgroundPageConnection = chrome.runtime.connect({
    name: "devtools-page"
});

backgroundPageConnection.onMessage.addListener(function (message) {
    // Handle responses from the background page, if any
});

// Relay the tab ID to the background page
chrome.runtime.sendMessage({
    tabId: chrome.devtools.inspectedWindow.tabId,
    scriptToInject: "content_script.js"
});
```
background page 中的代码：
```js
// Background page -- background.js
chrome.runtime.onConnect.addListener(function(devToolsConnection) {
    // assign the listener function to a variable so we can remove it later
    var devToolsListener = function(message, sender, sendResponse) {
        // Inject a content script into the identified tab
        chrome.tabs.executeScript(message.tabId,
            { file: message.scriptToInject });
    }
    // add the listener
    devToolsConnection.onMessage.addListener(devToolsListener);

    devToolsConnection.onDisconnect.addListener(function() {
         devToolsConnection.onMessage.removeListener(devToolsListener);
    });
}
```
#### 在调试窗口运行代码

你可以使用 [inspectedWindow.eval](https://developer.chrome.com/extensions/devtools_inspectedWindow#method-eval) 方法在 inspected page 的上下文环境中执行 JavaScript 代码。你可以在 Devtools 的页面、面板或侧边栏中调用 eval 方法。

默认情况下，表达式是在页面的主框架上下文中计算的。现在，你可能已经熟悉 Devtools 的命令行 API，如查看元素（inspect(elem)）、调试函数（debug(fn)）、操作剪贴板（copy()）等，inspectedWindow.eval() 与 Devtools console 中的代码使用的是相同的脚本执行上下文和选项，它可以让这些 API 在 eval 中使用。看下面的例子：

```js
chrome.devtools.inspectedWindow.eval(
      "inspect($$('head script[data-soak=main]')[0])",
      function(result, isException) { }
    );
```
另外，为 `inspectedWindow.eval()` 设置 `useContentScriptContext: true` 选项就可以让表达式和内容脚本使用同一个上下文对象。设置 useContentScriptContext: true 并不会创建一个脚本上下文，所以在调用 eval() 之前必须先加载上下文脚本，可以调用 executeScript，也可以在 mainfest.json 文件中指定一个内容脚本。

如果内容脚本已经存在，你可以使用这个选项来注入其他的内容脚本。

> 在正确的上下文中使用时，eval 是非常有用的，但是如果在不恰当的位置使用会非常危险。如果你是在不需要访问被检测页面的 JavaScript 上下文的情况下，可以使用 [ tabs.executeScript](https://developer.chrome.com/extensions/devtools) 方法。关于详细的注意事项以及二者的对比，可以查看 [inspectedWindow](https://developer.chrome.com/extensions/devtools_inspectedWindow)。

#### 把被选中的元素传入内容脚本

内容脚本不能直接访问当前被选元素。然而使用 [inspectedWindow.eval](https://developer.chrome.com/extensions/devtools_inspectedWindow#method-eval) 运行的代码可以访问 Devtools 控制台和命令行 API。比如，可以使用 $0 来访问被选中的元素。

向内容脚本中传递被选中元素：

- 在内容脚本中创建一个方法，一参数的形式传入被选中元素
- 在 Devtools page 中使用 inspectedWindow.eval 并配置 useContentScriptContext: true 参数来运行此方法

在内容脚本中定义方法：

```js
function setSelectedElement(el) {
    // do something with the selected element
}
```
在 Devtools page 调用方法：

```js
chrome.devtools.inspectedWindow.eval("setSelectedElement($0)",
    { useContentScriptContext: true });
```

因为 useContentScriptContext: true 选项指定表达式的上下文必须和内容脚本相同，所以它可以访问 setSelectedElement 方法。

#### 获取一个 reference 面板的窗口

先要从 Devtools 面板发送消息，你需要一个它的 window 对象的引用。从 [panel.onShown](https://developer.chrome.com/extensions/devtools_panels#event-ExtensionPanel-onShown) 时间的处理函数中获取一个面板的 iframe window：

```js
onShown.addListener(function callback)
extensionPanel.onShown.addListener(function (extPanelWindow) {
    extPanelWindow instanceof Window; // true
    extPanelWindow.postMessage( // …
});
```

#### 从内容脚本与 Devtools page 的通信：

Devtools page 与 内容脚本之间的通信是通过 background page 间接实现的。

background page 可以用 [tabs.sendMessage](https://developer.chrome.com/extensions/tabs#method-sendMessage) 方法传送一个消息到内容脚本，这个方法会把消息传送到一个指定栏的内容脚本中，与上面的 Injecting a Content Script 一样。

当从内容脚本发送一条信息时，没有现成的方法来传递消息到与当前选项卡关联的 Devtools page 示例。这里有一种变通的方法，可以让 Devtools page 与 background page 建立一个长时间连接，并且让 background page 与每个 tab ID 保持一个 map 来进行连接，这样它就可以路由每条信息到正确的连接。

```js
// background.js
var connections = {};

chrome.runtime.onConnect.addListener(function (port) {

    var extensionListener = function (message, sender, sendResponse) {

        // The original connection event doesn't include the tab ID of the
        // DevTools page, so we need to send it explicitly.
        if (message.name == "init") {
          connections[message.tabId] = port;
          return;
        }

	// other message handling
    }

    // Listen to messages sent from the DevTools page
    port.onMessage.addListener(extensionListener);

    port.onDisconnect.addListener(function(port) {
        port.onMessage.removeListener(extensionListener);

        var tabs = Object.keys(connections);
        for (var i=0, len=tabs.length; i < len; i++) {
          if (connections[tabs[i]] == port) {
            delete connections[tabs[i]]
            break;
          }
        }
    });
});

// Receive message from content script and relay to the devTools page for the
// current tab
chrome.runtime.onMessage.addListener(function(request, sender, sendResponse) {
    // Messages from content scripts should have sender.tab set
    if (sender.tab) {
      var tabId = sender.tab.id;
      if (tabId in connections) {
        connections[tabId].postMessage(request);
      } else {
        console.log("Tab not found in connection list.");
      }
    } else {
      console.log("sender.tab not defined.");
    }
    return true;
});
```

Devtools page（或面板和侧边栏）像这样建立连接：

```js
// Create a connection to the background page
var backgroundPageConnection = chrome.runtime.connect({
    name: "panel"
});

backgroundPageConnection.postMessage({
    name: 'init',
    tabId: chrome.devtools.inspectedWindow.tabId
});
```

#### 注入脚本与 Devtools page 的通信

上面的解决方案虽然适用于内容脚本，直接注入网页的代码（如使用 `<script>` 标签）使用的方却却有所不同，在这种情况下，[runtime.sendMessage](https://developer.chrome.com/extensions/runtime#method-sendMessage) 是不会向 background page 发送消息的。

你可以使用内容脚本作为媒介。你可以使用 [window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) API 来发送消息到内容脚本，这里有一个例子，假设使用上面的后台脚本：

```js
// injected-script.js

window.postMessage({
  greeting: 'hello there!',
  source: 'my-devtools-extension'
}, '*');
```

```js
// content-script.js

window.addEventListener('message', function(event) {
  // Only accept messages from the same frame
  if (event.source !== window) {
    return;
  }

  var message = event.data;

  // Only accept messages that we know are ours
  if (typeof message !== 'object' || message === null ||
      !message.source === 'my-devtools-extension') {
    return;
  }

  chrome.runtime.sendMessage(message);
});
```
现在，你的信息将会通过注入脚本到内容脚本，再到后台脚本，最后到达 Devtools page。

你还可以看一下 [messaging flow](https://github.com/GoogleChrome/devtools-docs/issues/143)

#### 检测 Devtools 的打开与关闭

如果你的扩展需要跟踪 Devtools 窗口的开启与关闭，你可以在 background page 添加 [onConnect](https://developer.chrome.com/extensions/runtime#event-onConnect) 监听器然后从 Devtools page 调用 [connect](https://developer.chrome.com/extensions/runtime#method-connect)。由于每个标签都有自己的 Devtools window open，所以你可能收到多个连接事件。如果想要确定是那些 Devtools 窗口是打开的，你需要计算连接和断开事件，如下：

```js
// background.js
var openCount = 0;
chrome.runtime.onConnect.addListener(function (port) {
    if (port.name == "devtools-page") {
      if (openCount == 0) {
        alert("DevTools window opening.");
      }
      openCount++;

      port.onDisconnect.addListener(function(port) {
          openCount--;
          if (openCount == 0) {
            alert("Last DevTools window closing.");
          }
      });
    }
});
```
 Devtools page 创建一个连接：

 ```js
// devtools.js

// Create a connection to the background page
var backgroundPageConnection = chrome.runtime.connect({
    name: "devtools-page"
});
```

Devtools 扩展示例：

浏览这些 Devtools 扩展的源文件：

- [Polymer Devtools Extension](https://developer.chrome.com/extensions/devtools) - 在主页上运行一些程序来查询 DOM/JS 状态并返回到自定义面板
- [React DevTools Extension](https://github.com/facebook/react-devtools) - 使用 Blink 的一个子模块来重用 Devtools 的 UI 组件
- [Ember Inspector](https://github.com/emberjs/ember-inspector) - 使用适配器共享 Chrome 和 Firefox 的扩展核心
- [Coquette-inspect](https://github.com/thomasboyt/coquette-inspect) - 在主页中注入一个调试代理的一个纯粹以 React 为基础的插件
- google 的 [扩展程序库](https://developer.chrome.com/devtools/docs/extensions-gallery) 

#### 更多资料

了解更多扩展可用的标准 API ，查看 [chrome.* APIs](https://developer.chrome.com/extensions/api_index)、[Other APIs](https://developer.chrome.com/extensions/api_other)。