# 铸造🍃:组件的事件和状态

> 原文：<https://dev.to/gdotdesign/mint-events-and-state-of-components-3j3a>

*这是展示 Mint 特性的系列文章的第三篇，你可以在这里找到之前的文章:*

*   *[薄荷🍃:入门](https://dev.to/gdotdesign/getting-started-with-mint-3k2o)T3】*
*   *[薄荷🍃:组件](https://dev.to/gdotdesign/components-in-mint-4a4n)T3】*

在这篇文章中，我将向你展示如何使用组件的**事件**和**内部状态【T3:)**

## 事件

每个 web 应用程序都利用事件来处理其状态的变化，这样的事件可能来自几个不同的来源:

*   来自 DOM 节点的用户交互
*   来自浏览器本身的交互
*   来自服务器的交互(例如来自 WebSocket 连接)
*   和可能的其他来源

我将向您展示如何处理来自 DOM 节点的事件。

### 事件属性和处理程序

与 React 中一样，DOM 节点可以附加事件属性:每个以上的**开始的属性都是一个**事件属性** ( `onClick`，`onMouseOver`，等等...)**

*由于 Mint 使用 React 作为平台，你可以参考它在[支持事件](https://reactjs.org/docs/events.html)列表上的文档。*

事件属性值是必须匹配以下两种类型之一的**函数**:

*   `Function(a)`返回`a`
*   `Function(Html.Event,a)`获取一个`Html.Event`并返回`a`。

*`a`是一个类型变量，这意味着它可以是任何其他类型。*

例如，在本例中，两个处理程序都有效:

```
component Main {
  fun increment (event : Html.Event) : String {
    Debug.log("Increment")
  }

  fun decrement : String {
    Debug.log("Decrement")
  }

  fun render : Html {
    <div>
      <button onClick={decrement}>
        "Decrement"
      </button>

      <button onClick={increment}>
        "Increment"
      </button>
    </div>
  }
} 
```

Enter fullscreen mode Exit fullscreen mode

当点击一个按钮时，你会在控制台上看到`increment`或`decrement`，这取决于你点击了哪个按钮。

如你所见，你可以引用函数本身，而不用调用它，只需通过它的名字。

### Html。事件

`Html.Event`类型是 [DOM 事件](https://developer.mozilla.org/en-US/docs/Web/API/Event)接口的规范化版本，你可以在这里看到实际的类型定义[。](https://github.com/mint-lang/mint/blob/master/core/source/Html.Event.mint#L2)

## 内部状态

组件可以有自己的状态来实现某些不需要全局状态的特定功能。

一个**状态**可以使用`state`关键字定义，类似于`property`关键字:

```
component Main {
  state count : Number = 0

  ...
} 
```

Enter fullscreen mode Exit fullscreen mode

这个**状态**通过引用就可以跨组件使用:

```
 ...
      <button onClick={decrement}>
        "Decrement"
      </button>

      <{ Number.toString(count) }>

      <button onClick={increment}>
        "Increment"
      </button>
  .... 
```

Enter fullscreen mode Exit fullscreen mode

### 修改状态

可以使用`next`关键字设置一个状态(或多个状态):它告诉组件用新值替换给定的状态。

状态不会变异，但会被替换，因为 Mint 中的数据结构是不可变的。

例如，我们修改函数来更新计数:

```
...
  fun increment : Promise(Never, Void) {
    next { count = count + 1 }
  }

  fun decrement : Promise(Never, Void) {
    next { count = count - 1 }
  }
... 
```

Enter fullscreen mode Exit fullscreen mode

请注意，函数的返回类型已经更改为`Promise(Never, Void)`。

承诺用于异步计算(我们将在下一篇文章中讨论)，修改状态会返回一个承诺，因为这被认为是副作用。

一个承诺有两个参数，第一个是错误类型，在这种情况下它是`Never`表示它不能失败，第二个是当它解析时返回值的类型，在这种情况下它是`Void`表示它不相关(基本上不能用于任何事情)。

* * *

这里是完整的源代码，感谢您的阅读🙏:

```
component Main {
  state count : Number = 0

  fun increment : Promise(Never, Void) {
    next { count = count + 1 }
  }

  fun decrement : Promise(Never, Void) {
    next { count = count - 1 }
  }

  fun render : Html {
    <div>
      <button onClick={decrement}>
        "Decrement"
      </button>

      <{ Number.toString(count) }>

      <button onClick={increment}>
        "Increment"
      </button>
    </div>
  }
} 
```

Enter fullscreen mode Exit fullscreen mode

* * *

如果你想了解更多关于 Mint 的知识，请查看[指南](https://guide.mint-lang.com)📖

在下一部分中，我将向[展示如何从 JSON API](https://dev.to/gdotdesign/mint-handling-http-requests-2ep3) 加载和显示数据😉在那里见👋