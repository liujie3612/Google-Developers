## Chrome 调试协议客户端的展示

有大量使用 Chrome 调试协议的第三方客户端，这部分就来展示一部分。

#### Brackets

Brackets 是一个 web-based IDE，使用了 Chrome 调试协议来提供调试和现场编辑。

![brackets](../../../images/brackets.png)

- 下载 [Brackets ](http://download.brackets.io/)，查看源码：[github](https://github.com/adobe/brackets)
- [blog post from Mark DuBois](http://www.markdubois.info/weblog/2013/03/adobe-brackets-revisited/) 有一个使用 Brackets 的概览

#### DevTools App

[DevTools App](https://chrome.google.com/webstore/detail/dev-tools-app/eichfopopofffkbhjgbabdegakcdmpkm) 是一个 Chrome 应用，它可以让你非常简单地尝试不同版本的 Devtools。

![devtoolsapp](../../../devtoolsapp.png)

- 查看源码 [github](https://github.com/sandipchitale/DevToolsApp)

#### Light Table

Light Table 是一个新的 IDE，它使用了一个新方法来分配开发者的工作区。现在它还是 Alpha 版本。没有开放源码，但是现在的 alpha 版本允许免费使用。

![lighttable](../../../images/lighttable.png)

- 从[官方网站](http://www.lighttable.com/)下载
- 查看 0.4.0 版本的[介绍](http://www.chris-granger.com/2013/04/28/light-table-040/)，其中包括整合 Devtools

#### NodeJS

Node 脚本中已经有很多使用了 Chrome 调试工具的模块。

<strong>chrome-remote-interface</strong>

chrome-remote-interface 使用一个 Node-style JavaScript API 封装了 debugger 协议。

```
npm install -g chrome-remote-interface
```

![chrome-remote](../../../images/chrome-remote.png)

查看那些 NPM 项目使用了 chrome-remote-interface ：[which NPM projects use chrome-remote-interface](https://www.npmjs.com/browse/depended/chrome-remote-interface)。

<strong>crconsole</strong>

[crconsole](https://github.com/sidorares/crconsole) 模块提供了 Chrome 控制台的命令行。它使用 chrome-remote-interface 来与 Chrome debugger protocol 沟通。

<strong>automated-chrome-profiling</strong>

利用 Node.js 来自动分析 js 的一个基础框架。查看协议生态系统中的 [other apps mentioned](https://github.com/paulirish/automated-chrome-profiling/blob/master/readme.md#way-more-is-possible)

<strong>chrome-debug-protocol</strong>

chrome-debug-protocol 适用于 JavaScript 和 TypeScript。可以让 Chrome 中的自动化和分析易于实现。

<strong>Sublime Text</strong>

Sublime Web Inspector project 将 Chrome debugger 与 Sublime Text 编辑器整合起来，你可以在 Sublime Text 的 package manager 中安装它。

![sublime](../../../images/sublime.png)

- 查看[官方文档](http://sokolovstas.github.io/SublimeWebInspector/)来了解插件以及安装说明
- 产看源代码：[github](https://github.com/sokolovstas/SublimeWebInspector)

<strong>Telemetry</strong>

Telemetry 是一个性能测试框架，Chromium 项目使用它来测试多个版本的 Chrome 浏览器。它使用调试协议来远程控制 Chrome 实例。

- [Introduction to Telemetry on Chromium.org](http://www.chromium.org/developers/telemetry)

<strong>Vim</strong>

Chrome.vim 是 Vim 编辑器的一个实验性的插件，它将一些实用的 Chrome 操作提供到了 Vim 命令行中。

- [vim-chrome on Github](https://github.com/mklabs/vimfiles/tree/master/custom-bundle/vim-chrome)


<strong>WebDriver</strong>

Selenium 浏览器自动化测试工具就是使用 WebDriver API 来提取各种浏览器之间的不同之处，WebDriver 是使用 Chrome 调试协议实现的。

<strong>WebStorm</strong>

WebStorm 是一个收费 IDE，支持在 Chrome 中调试和实时编辑。WebStorm 使用了一个 Chrome 扩展来整合 Chrome 调试器。

![webstorm](../../../images/webstorm.png)

- 下载：[JetBrains](http://www.jetbrains.com/webstorm/)
- 最新调试功能[视频](https://www.youtube.com/user/JetBrainsTV)

<strong>Python</strong>

[chrome_remote_shell](https://github.com/minektur/chrome_remote_shell)为 Python 应用提供了一个很好的 API 层