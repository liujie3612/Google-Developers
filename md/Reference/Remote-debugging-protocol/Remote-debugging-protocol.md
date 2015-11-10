## 远程调试协议

按照原理来说，Chrome Devtools 是一个使用 HTML、JavaScript、CSS 写出来的 web 应用程序。它有一个特殊的绑定来让运行中的 JavaScript 与 Chrome 页面进行交和调试。交互协议由发送到页面中的命令以及页面生成的事件组成。虽然 Chrome Devtools 是这项协议的主要客户端，在使用 [remote debugging](../../../Use-Tools/remote-debugging.md) 之后，就可以让第三方使用它并且调试浏览器页面。下面我们将描述这些方法。

#### 协议

交互协议由发送到也面的 JSON 命令和页面生成的事件组成。我们在 Blink 中定义它（"upstream"），所以任何 Blink-based 的浏览器都会支持它。

<strong>稳定</strong>

[Debugger protocol version 1.1](version-1.1.md) 是协议的最新稳定版本。

Chrome 31 版本已经支持 1.1 版本的协议。所有 1.* 以上的版本都会对 1.1 版本做向后兼容。我们的向后兼容原则：

- 原来的协议命令及时间不会被移除
- 命令所需的参数不会增加
- 命令响应及事件的参数都不会移除

以前的版本：[Protocol v1.0](version-1.0.md) 从 Chrome 18 开始支持，[Protocol v0.1](version-0.1.md) 从 Chrome 16开始支持。

<strong>起源</strong>

[tip-of-tree protocol](https://code.google.com/p/chromium/codesearch#chromium/src/third_party/WebKit/Source/devtools/protocol.json&q=protocol.json&sq=package:chromium&type=cs)是不稳定的，并且可能随时被中断。虽然它拥有协议的所有功能，而稳定版本只是它的一个子集。它引入的功能不能保证向后兼容，你可以使用它与[Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)构建，但是需要自己承担风险。查看下面的协议部分来确定 Chrome 中使用的协议命令。

[Chrome debugger protocol viewer](http://chromedevtools.github.io/debugger-protocol-viewer/)为 tip-of-tree 协议提供了更多可读视图。

[tip-of-tree protocol](https://chromium.googlesource.com/chromium/src/+log/master/third_party/WebKit/Source/devtools/protocol.json)会经常变化，如果你需要某个特定的 Chrome 客户端的协议版本，这个[片段](https://github.com/cyrus-and/chrome-remote-interface/issues/10#issuecomment-146032907)将会提供基于客户端 /json/version 的协议 JSON。

#### 在线调试

Developer Tools front-end 可以与一个正在运行的远程 Chrome 实例连接来进行调试。这种情景下，你需要使用 remote-debugging-port 命令行来打开主机 Chrome 实例：

```
chrome.exe --remote-debugging-port=9222
```

现在你就可以使用不同的用户配置来打开一个独立的 Chrome 实例的客户端：

```
chrome.exe --user-data-dir=<some directory>
```

现在你就可以从客户端导航到定义的端口并且链接到任何显示的栏来进行调试：[http://localhost:9222](http://localhost:9222)

你会发现 Developer Tools 的界面与浏览器内置的 Devtools 是一样的，原因如下：

- 当你将你的客户端浏览器导航至远程的 Chrome 端口，Developer Tools front-end 是主机 Chrome 从 Web Server 起的一个 Web 应用程序服务
- 它通过 HTTP 来获取 HTML、JavaScript 和 CSS 文件
- 一旦加载完成， Developer Tools 会建立一个 Web Socket 连接到它的主机并开始传递 JSON 消息

在这种情况下，完全可以用 Developer Tools front-end 来而不用自己实现。你的应用可以访问 http://localhost:9222/json 来获取可用的页面，访问之后会获得一个 JSON 对象，此对象包含可以使用 WebSocket 地址调试的页面的信息，这些页面可以直接在浏览器打开。

远程调试在调试远程浏览器实例或者附加到嵌入设备时是非常有用的。Blink 端口的所有者负责暴露调试连接给外部的用户。


#### 查看协议

你可以查看 Chrome 是如何使用协议的，在查找新功能时，这是非常方便的。首先，在打开 Chrome 的同时，开启调试端口：

```
/Applications/Google\ Chrome\ Canary.app/Contents/MacOS/Google\ Chrome\ Canary --remote-debugging-port=9222 http://localhost:9222 http://chromium.org
```
然后，在出现的调试页面的列表中选择 Chromium Projects 项，现在的 Devtools 是全屏打开的，打开 Devtools 进行调试。Cmd+R 来对新的调试器进行刷新，现在调到 Network 面板，选择过滤类型为 Websocket，选择连接并点击框架标签。现在，你可以很轻松地看到你第一次使用 Devtools 实例中 WebSocket 活动的框架。

[debugger-protocol-sniffing-full](../../../images/debugger-protocol-sniffing-full.png)

#### 调试协议客户端

很多应用和库都已经使用了这个协议。一些用来收集性能数据，另外的用来从另一个编辑器调试断点。有很多 Nodejs 和 Python 的用来访问原生协议的库。

#### 使用调试器扩展 API

想要让第三方与协议交互，我们推出 chrome.debugger 扩展 API，开放此消息传输接口。这样一来，你不仅可以使用 Chrome 实例来与远端连接，还可以将此功能用在自己的扩展中。

Chrome Debugger Extension API 在 sendCommand 调用时提供的 command domain、name、body上提供了很高权限的 API。这个 API 会隐藏请求 ID并将请求与其响应绑定，因此，允许 sendCommand 在回调函数调用时使用报告的结果。你也可以结合其他扩展 API 来使用这个 API。

如果你正在开发一个 Web-based IDE，你需要实现一个扩展来在你的页面使用调试功能，然后你的 IDE 就可以打开目标应用的页面了，可以实现 Devtools 的所有功能。

打开内嵌的 Devtools 会中断远程连接并分离扩展。

#### 并存协议客户端

我们现在并不支持多个客户端同时连接到同一个协议。包括开启 Devtools 而另一个客户端也在连接。在 bug 跟踪系统，[crbug.com/129539](https://code.google.com/p/chromium/issues/detail?id=129539) 提出了这一问题，你可以标记星标，让每次更新都得到通知。

一旦断开，外部的客户端将会接收到一个退出事件，如：{"method":"Inspector.detached","params":{"reason":"replaced_with_devtools"}}。查看可能的[原因](https://code.google.com/p/chromium/codesearch#chromium/src/out/Debug/gen/chrome/common/extensions/api/debugger.cc&q=file:debugger.cc%20Reason%20ParseReason&sq=package:chromium&type=cs&l=138)。中断之后，一些应用会暂停它们的状态并出现一个重连按钮。