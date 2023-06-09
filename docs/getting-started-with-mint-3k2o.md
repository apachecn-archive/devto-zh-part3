# 铸造🍃:开始使用

> 原文：<https://dev.to/gdotdesign/getting-started-with-mint-3k2o>

Mint 是一种令人耳目一新的现代网络编程语言(我是它的开发者)。

这是介绍 Mint 及其特性系列的第一篇文章。

## 为什么用薄荷？

下面的列表应该会提供一个不错的激励:)

*   强类型、类似 JavaScript 的语法
*   不可变的数据结构和函数式编程元素
*   HTML 元素和组件的类 HTML 语法
*   包括一切:
    *   按指定路线发送
    *   对组件的支持
    *   使用 CSS 设置样式
    *   标准程序库
    *   数据存储(如 Redux)
    *   开发服务器
    *   格式程序
    *   环境变量处理
    *   测试跑步者
    *   文档服务器
    *   JavaScript 互操作性
    *   渐进式 Web 应用程序支持
*   优化输出(缩小、损坏)
*   [死码消除](https://en.wikipedia.org/wiki/Dead_code_elimination)
*   包含整个工具链的单个二进制文件
*   使用反应平台
*   和更多令人敬畏的功能...

## 安装

Mint 以一个二进制文件的形式出现:`mint`。要安装它，请遵循安装页面上的说明(基本上是下载二进制文件并将其添加到路径中)。

## 创建新项目

一旦你安装了 Mint，你就可以用`mint init`命令:
创建一个新项目

```
mint init my-awesome-project 
```

Enter fullscreen mode Exit fullscreen mode

如果成功，您应该会看到:

```
Mint - Initializing a new project
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚙ Creating directory structure...
⚙ Writing initial files...

There are no dependencies!

There is nothing to do!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
All done in 2.231ms! 
```

Enter fullscreen mode Exit fullscreen mode

这将创建以下目录/文件结构:

```
my-awesome-project
├── source
│   └── Main.mint
├── tests
│   └── Main.mint
├── .gitignore
└── mint.json 
```

Enter fullscreen mode Exit fullscreen mode

## 开发服务器

Mint 带有一个内置的开发服务器，当发生变化时，它会重新编译代码(并重新加载浏览器),进入项目目录并运行`mint start`命令:

如果成功，您应该会看到:

```
Mint - Running the development server
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚙ Ensuring dependencies... 181μs
⚙ Parsing files... 2.608ms
⚙ Development server started on http://127.0.0.1:3000/ 
```

Enter fullscreen mode Exit fullscreen mode

现在你可以在`http://127.0.0.1:3000/`或者`http://localhost:3000/`打开正在运行的项目。

如果项目正在运行，您应该会看到以下内容:

[![](img/ff9cbd59a3c6044e142e9443eb1699d2.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--vpmkXAlJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/dyvfbhcu61y25n3aibs3.png)

现在你已经在当地启动并运行了一个造币厂项目🎉

## 在线游乐场

如果你只是想瞎折腾不安装，可以使用网站上的[试用页面](https://www.mint-lang.com/try)。

如果你想了解更多关于 Mint 的知识，请查看[指南](https://guide.mint-lang.com)📖

在下一部分中，我将展示如何创建组件😉在那里见👋