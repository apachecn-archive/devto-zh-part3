# 铸造🍃:环境变量

> 原文：<https://dev.to/gdotdesign/mint-environment-variables-24gf>

*这是展示 Mint 特性系列的下一篇文章，你可以在这里找到之前的文章:*

*   *[薄荷🍃:入门](https://dev.to/gdotdesign/getting-started-with-mint-3k2o)T3】*
*   *[薄荷🍃:组件](https://dev.to/gdotdesign/components-in-mint-4a4n)T3】*
*   *[薄荷🍃:组件的事件和状态](https://dev.to/gdotdesign/mint-events-and-state-of-components-3j3a)*
*   *[薄荷🍃:处理 HTTP 请求](https://dev.to/gdotdesign/mint-handling-http-requests-2ep3)T3】*
*   *[薄荷🍃:造型元素](https://dev.to/gdotdesign/mint-styling-elements-295o)*
*   *[薄荷🍃:创建包](https://dev.to/gdotdesign/mint-creating-a-packages-5e9b/)*
*   *[薄荷🍃:路由](https://dev.to/gdotdesign/mint-routing-2h69/)*

在这篇文章中，我将向你展示如何使用环境变量。

* * *

在任何应用程序中，能够定义特定于[部署环境](https://en.wikipedia.org/wiki/Deployment_environment)的变量是必要的。假设您可能希望在开发期间连接到本地 API 端点，而在生产时连接到远程 API。

### 定义环境变量

Mint 使用`.env`文件来存储特定于环境的变量，通常是这样的:

```
ENDPOINT=http://localhost:3001
WSENDPOINT=ws://localhost:3001
GATRACKINGID=google-analytics-tracking-id 
```

Enter fullscreen mode Exit fullscreen mode

这里我们声明了三个变量`WSENDPOINT`、`ENDPOINT`和`GATRACKINGID`，我们希望在我们的代码中使用它们。

### 使用环境变量

在 Mint 中，您可以使用 at ( `@`)符号，后跟变量名称来引用它:

```
module Main {
  fun render : Html {
    <div>
     <{ @ENDPOINT }>
    </div>
  }
} 
```

Enter fullscreen mode Exit fullscreen mode

本质上，变量值将在编译期间用类型`String`内联。

在另一个例子中，你可以看到在提出请求时如何使用它:

```
...

response =
 @ENDPOINT + "/api/planets"
 |> Http.get()
 |> Http.send()

... 
```

Enter fullscreen mode Exit fullscreen mode

如果应用程序中没有定义环境变量，则会显示一条很好的错误消息:

[![Error](img/52d769b76e3792a3bb2894970791dcfe.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--BE2giT5y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ff6g2spc8eyy8i37rmfr.png)

### 使用不同的`.env`文件

默认情况下，加载应用程序根目录下的`.env`文件，但是您可以使用`--env`(或`-e`)标志指定一个不同的文件，如下所示:

```
mint build --env .env.production 
```

Enter fullscreen mode Exit fullscreen mode

今天就到这里，谢谢你的阅读🙏

* * *

如果你想了解更多关于 Mint 的知识，请查看[指南](https://guide.mint-lang.com)📖

在下一部分，我将告诉你关于商店的事情😉在那里见👋