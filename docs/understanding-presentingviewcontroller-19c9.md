# 了解 iOS 中的 presentingViewController

> 原文：<https://dev.to/onmyway133/understanding-presentingviewcontroller-19c9>

## 呈现(_:动画:完成:)

[https://developer . apple . com/documentation/ui kit/uiview controller/1621380-present](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621380-present)

> 调用此方法的对象可能并不总是处理表示的对象。每种演示风格都有不同的行为规则。例如，全屏显示必须由一个视图控制器来完成，它本身覆盖了整个屏幕。如果当前视图控制器无法完成请求，它会将请求沿视图控制器层次结构向上转发到最近的父视图控制器，然后父视图控制器可以处理或转发请求。

假设我们试图将 viewController2 呈现给 viewController1

```
vc1.present(v2, animated: true, completion: nil) 
```

在这之后`vc1.presentedViewController == vc2`。你也可能认为`vc2.presentingViewController == vc1`但情况并非总是如此。
当一个呈现视图控制器发生时，游戏中可能有 3 个玩家

### 呈现视图控制器

这是 vc2

### 原演示者

这是 vc1，有时也称为源代码视图控制器。在这个阶段，`vc1.presentedViewController` == vc2

### 呈现视图控制器

这是`vc2.presentingViewController`，但并不总是`vc1`。这是视图控制器，其视图被`vc2.view`替换或覆盖。默认情况下，它是视图占据整个屏幕的视图控制器，它可以是根视图控制器，也可以是已经呈现的视图控制器。它可能与 vc1 不同。

例如，呈现视图控制器是根视图控制器，我们有`rootViewController.presentedViewController == vc2`。所以在这个例子中，vc2 可以是原始呈现者(vc1)和呈现视图控制器(根视图控制器)的`presentedViewController`

如果 vc1 占据整个屏幕，则原始演示者和演示视图控制器是相同的(vc1)

在另一个世界里，vc2.presentingViewController 只会指向真正的呈现视图控制器。

## 解散(动画:完成:)

[https://developer . apple . com/documentation/ui kit/uiview controller/1621505-解散](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621505-dismiss)

我们可以向这 3 个对象中的任何一个发送`dismiss(animated:completion:)`,系统会将该消息转发给呈现视图控制器。

继续 rootViewController 作为真正的呈现视图控制器的例子，如果你说

```
vc2.dismiss(animated: true, completion: nil) 
```

那么根视图控制器就是接收这个消息的那个。

## 呈现视图控制器链

一个视图控制器一次最多可以有 1 个`presentedViewController`。然而，一个呈现的视图控制器(vc2)本身可以呈现另一个视图控制器(vc3)

```
vc2.present(vc3, animated: true, completion: nil) 
```

## 演示上下文

那么“占满屏幕”是什么意思呢？阅读[呈现上下文提供被呈现的视图控制器覆盖的区域](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ModalViewControllers/ModalViewControllers.html)

> 用于定义演示区域的屏幕区域由演示上下文决定。默认情况下，表示上下文由根视图控制器提供，其框架用于定义表示上下文的框架。但是，呈现视图控制器或视图控制器层次结构中的任何其他祖先可以选择提供呈现上下文。在这种情况下，当另一个视图控制器提供呈现上下文时，它的框架被用来确定所呈现视图的框架。这种灵活性允许您将模式呈现限制在屏幕的较小部分，而让其他内容可见。
> 
> 当显示视图控制器时，iOS 会搜索显示上下文。它从呈现视图控制器开始，读取它的 definesPresentationContext 属性。如果该属性的值为 YES，则表示视图控制器定义表示上下文。否则，它继续向上通过视图控制器层次结构，直到一个视图控制器返回 YES，或者直到它到达窗口的根视图控制器。
> 
> 当视图控制器定义表示上下文时，它也可以选择定义表示样式。通常，呈现的视图控制器使用其 modalTransitionStyle 属性确定其呈现方式。将 definesPresentationContext 设置为 YES 的视图控制器也可以将 providesprentationcontexttransitionstyle 设置为 YES。如果 providesPresentationContextTransitionStyle 设置为 YES，iOS 将使用表示上下文的 modalPresentationStyle 来确定新视图控制器的显示方式。

### 定义 PresentationContext

[https://developer . apple . com/documentation/ui kit/uiview controller/1621456-definepresentationcontext](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621456-definespresentationcontext)

> 当呈现一个视图控制器时，iOS 从呈现视图控制器开始，询问它是否想要提供呈现上下文。如果呈现视图控制器没有提供上下文，那么 iOS 会询问呈现视图控制器的父视图控制器。iOS 在视图控制器层次结构中向上搜索，直到视图控制器提供一个表示上下文。如果没有视图控制器提供上下文，那么窗口的根视图控制器提供表示上下文。
> 
> 如果视图控制器返回 YES，那么它提供一个表示上下文。视图控制器的视图所覆盖的窗口部分决定了所呈现的视图控制器视图的大小。该属性的默认值是“否”

## 阅读更多

*   [展示视图控制器](https://useyourloaf.com/blog/presenting-view-controllers/)
*   [第十九章。视图控制器-呈现视图控制器](http://www.apeth.com/iOSBook/ch19.html#_presented_view_controller)

* * *

支持我的应用程序

*   [推送 Hero -测试推送通知的纯 Swift 原生 macOS 应用](https://onmyway133.com/pushhero)
*   [PastePal -粘贴板、便笺和快捷方式管理器](https://onmyway133.com/pastepal)
*   [快速检查-智能待办事项管理器](https://onmyway133.com/quickcheck)
*   [Alias - App 和文件快捷方式管理器](https://onmyway133.com/alias)
*   [我的其他应用](https://onmyway133.com/apps/)

❤️❤️😇😍🤘❤️❤️