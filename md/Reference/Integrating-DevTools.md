## 集成 Devtools

Chrome Devtools 在开发时是可以扩展的。所，当使用 Devtools 时，如果缺少你需要的功能，你可以找一个现有的扩展或者自己写一个，甚至你可以把 Devtools 集成到应用程序中。

有两种方法可以在 Devtools 中自定义功能：

- DevTools Extension：一个 Chrome 扩展，可以在 Devtools 中添加插件并扩展其 UI 界面
- Debugging Protocol Client：Chrome 支持的一个使用 Chrome 远程调试协议插入低级调试的第三方应用

下面讨论的就是这两种方法。

#### DevTools Chrome扩展

Devtools UI 界面是嵌入到 Chrome 中的一个 web 应用。Devtools 扩展通过 Chrome 扩展系统来添加功能。一个 Devtools 扩展可以在正在被检查的浏览器选项卡中增加一个面板、在 Elements 或 Sources 面板的侧边栏添加新栏、检查资源和网络事件，以及调试 JavaScript 表达式。

如果你想开发一个 Devtools 扩展：

- 如果你没有开发过 Chrome 扩展，可以查看 [Overview of Chrome Extensions](https://developer.chrome.com/extensions/overview)
- 可以查看[Extending DevTools](https://developer.chrome.com/extensions/devtools)来了解创建 Chrome Devtools 扩展的相关细节

#### 调试协议客户端

第三方应用程序，如：集成开发环境、编辑器、持续集成工具和测试框架都可以集成 Chrome 调试器来调试代码、预览、CSS 变化，还可以控制浏览器。客户端使用 Chrome 调试协议来和一个 Chrome 实例连接，它可以同时同一系统或远程运行。

目前，Chrom 的调试协议支持每个页面只能有一个客户端。所以，你可以使用 Devtools 检查一个页面，或者使用第三缸客户端，但二者不能同时使用。

现在有两种方法来使用调试协议来集成：

- 当应用运行在 Chrome 上时，可以使用调试器模块化一个 Chrome 扩展，[chrome.debugger](https://developer.chrome.com/extensions/debugger)。该模块可以让扩展不使用 Devtools UI 界面与调试器直接交互。
- 其他应用可以使用 wire protocol 来直接使用调试器，该协议涉及通过 WebSocket 连接来交换 JSON 数据

