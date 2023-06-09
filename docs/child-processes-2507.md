# 子进程👶

> 原文：<https://dev.to/ruheni/child-processes-2507>

# 子进程

对于本文，假设您对 NodeJs 和 JavaScript 中级编程知识有基本的了解。为了能够牢牢掌握子进程，您需要对流和事件发射器有一个相当好的理解。我要做的最后一个假设是，你对用于操作文件和文件夹的终端命令有基本的了解。

## 到底什么是子进程？

子进程是由另一个进程创建的进程。听起来很 meta 吧…？重要的是要记住 JavaScript 是单线程的，因此我们不希望阻塞主线程的运行，否则我们的应用程序将会很慢，它将不能做很多事情。万一你问“他到底在说什么…？”😕先别慌。

[JavaScript 事件循环](https://www.youtube.com/watch?v=cCOL7MC4Pl0)解释了 JavaScript 在执行时到底是如何运行的😉。

使用子进程，您可以无缝地运行多个进程，并提高应用程序的性能。您可以控制输入流，监听它的输出流，并将它传递给另一个进程，这与管道操作符在 Linux 中的工作方式相同

子进程使您能够通过在操作系统中运行子进程来访问操作系统功能。创建子进程有四种不同的方式，它们在速度(有些比其他的稍微更有效)和功能上有所不同。您可以通过以下方式创建子流程:`fork()`、`spawn()`、`exec()`和`execFile()`。

要访问这个模块，你必须将它导入到你的程序中，并对它进行析构，以访问你想要使用的特定子进程。我将在本文中使用的例子相当简单😁。

由于我不想让这个教程太长，我将只涉及`spawn()`。

```
const { spawn, exec, execFile, fork } = require("child_process"); 
```

Enter fullscreen mode Exit fullscreen mode

## 产卵

这将在新进程中启动新命令。使用 spawn，您可以向它传递任何参数来执行给定的过程。`spawn`能够处理事件，也就是说，您可以注册事件，以便在发送信号时执行给定的任务。

使用`spawn()`时，可以记录以下事件:

1.  `error` -当进程不能产生或终止时发出。
2.  `message` -当孩子使用`process.send()`发送消息时发出。它支持子进程和父进程之间的通信。
3.  `disconnect` -当父进程手动调用`child.disconnect()`时
4.  `close` -当给定进程的输入和输出流关闭时发出。
5.  `data`—`data`事件在给定的流可读时发出，我们想要操作输入和输出数据。它也可以在出错时发出。

*对于那些不熟悉`pwd`命令的人来说，它用来显示当前的工作目录。*T3】

```
const { spawn } = require("child_process");
const pwd = spawn("pwd");

pwd.stdout.on("data", data => console.log(`path: ${data}`));

pwd.stderr.on("data", data => {
  console.error(`child stderr: ${data}`);
});

pwd.on("exit", (code, signal) => {
  console.log(`child process exited with ${code} and signal ${signal}`);
}); 
```

Enter fullscreen mode Exit fullscreen mode

输出:

```
path: "/c/Users/Users/Documents/pandora's_box/sandbox/nodejs"
child process exited with 0 and signal null 
```

Enter fullscreen mode Exit fullscreen mode

可选地，`spawn`函数能够接受第二个参数，该参数是包含字符串的数组，这些字符串是在执行给定命令时想要使用的参数。

```
const { spawn } = require("child_process");
const py = spawn("py", ["--version"]);

py.stdout.on("data", data => console.log(`python version: ${data}`));

py.stderr.on("data", data => {
  console.error(`child stderr: ${data}`);
});

py.on("exit", (code, signal) => {
  console.log(`child process exited with ${code} and signal ${signal}`);
}); 
```

Enter fullscreen mode Exit fullscreen mode

输出:

```
python version: Python 3.7.3
child process exited with 0 and signal null 
```

Enter fullscreen mode Exit fullscreen mode

*输出将随您当前的工作目录而变化，代码 0 表示在执行程序时没有遇到错误...*

*快乐黑客🎉😁*