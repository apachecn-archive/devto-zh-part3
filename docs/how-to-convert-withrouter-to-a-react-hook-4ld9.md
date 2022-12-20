# 如何将 withRouter 转换成 React 钩子？

> 原文：<https://dev.to/charlesstover/how-to-convert-withrouter-to-a-react-hook-4ld9>

新版本 16.7 中 React 挂钩的最大好处之一是消除了对高阶组件的依赖。在将我的类状态移植到函数钩子的过程中，我很高兴有机会将我的[和-router](https://github.com/CharlesStover/with-router/blob/master/README.md) 高阶组件移植到一个钩子上。我一直在使用这个定制的实现来重新呈现我的组件。`react-router`的内置`withRouter` HOC 不会在路线改变时重新渲染组件，这是一个[难以解决的问题](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/blocked-updates.md)。

这篇文章应该作为实现具有发布-订阅功能的 React 挂钩的教程。Pub-sub 是“发布-订阅”的缩写，是一种编程方法，其中当更新发布时，一组订阅进程得到通知。简而言之，当位置改变时(发布的事件)，我想重新呈现我的组件(侦听器订阅)。

最终产品需要 React Router v4.4 或更高版本，因为 React Router 的早期版本不公开路由器上下文，我们使用它来侦听位置变化。如果您无法访问 React 路由器的最新版本，您可以收听窗口历史状态作为替代方法。我选择使用与`react-router`相同的“真实的单一来源”,确保我的状态总是同步的。

你可以在 NPM 上通过 [use-react-router 自己使用这个包，或者贡献给](https://www.npmjs.com/package/use-react-router)[开源 GitHub 库](https://github.com/CharlesStover/use-react-router)，或者以其他方式窥探它。

# 用户体验🙃

我开始每个项目时都会问这样一个问题，“我想做什么？”这并不意味着，“我想如何实现这个特性？”这意味着，作为一名开发人员，我多么希望这个特性已经实现了。我希望*和*做些什么才能在将来使用该功能？如何让*直观*和*好用*？

幸运的是，`withRouter`不是一个困难的实现，所以它的钩子也不是。

我想从默认的包导出中导入一个钩子。

```
import useReactRouter from 'use-react-router'; 
```

Enter fullscreen mode Exit fullscreen mode

我想调用那个钩子，然后*结束它*。

```
const { history, location, match } = useReactRouter(); 
```

Enter fullscreen mode Exit fullscreen mode

这将我的组件中的历史、位置和比赛道具整理出来，并允许我挑选我想要存在的。

我不希望必须实现进一步的逻辑来重新渲染位置的变化。我希望该功能已经实现，这与`react-router`的特设实现形成鲜明对比。那只是个人喜好，你可能反而会赞同他们的实现。本教程将讨论如何实现这两者，因为这是一个常见的特性请求，并且与我的个人用例高度相关。

# 实现

## 从属关系👶

首先，我们的依赖。

我们将需要来自`react-router`包的路由器上下文，因为它包含我们想要在组件中访问的数据，并且我们将需要来自 react 包的`useContext`钩子来“挂钩”路由器上下文。因为我们正在实现发布-订阅功能，所以我们还需要 React 内置的`useEffect`挂钩。这将允许我们订阅和取消订阅的位置变化。

```
import { useContext, useEffect } from 'react';
import { __RouterContext } from 'react-router'; 
```

Enter fullscreen mode Exit fullscreen mode

最后，我将从 [`use-force-update` NPM 包](https://www.npmjs.com/package/use-force-update)中导入`useForceUpdate`。这仅仅是调用`useState`钩子来强制重新渲染的简写，当位置改变时我们将会这样做。

```
import useForceUpdate from 'use-force-update'; 
```

Enter fullscreen mode Exit fullscreen mode

## 钩子🎣

导入依赖项后，我们可以开始编写钩子

```
const useReactRouter = () => {
  const forceUpdate = useForceUpdate();
  const routerContext = useContext(__RouterContext);
  /* TODO */
  return routerContext;
}; 
```

Enter fullscreen mode Exit fullscreen mode

我们首先实例化我们将需要的所有其他钩子。现在是一个函数，当被调用时，重新呈现组件。`routerContext`现在是`react-router`上下文的内容:一个具有`history`、`location`和`match`属性的对象——与您期望从`withRouter`获得的道具完全相同。

**如果您不想要重新渲染功能，**您可以在此停止。您可以删除`forceUpdate`变量、`useEffect`导入和[T2 依赖](https://www.npmjs.com/package/use-force-update)。我建议在您的组件中使用一个外部的`useReactRouter`钩子来调用`useContext`,仅仅是因为`__RouterContext`名称和`@next` semvar 当前需要访问 React Router v4.4。访问这个上下文可能会发生变化，在单个包中进行调整比在您的项目中使用的每个依赖于路由器的组件中进行调整要简单得多。对于开发人员来说,`useReactRouter`比`useContext(__RouterContext)`更加直观——额外的上下文导入是多余且不变的。

## Pub-Sub📰

为了实现发布-订阅行为，我们需要`useEffect`。这将允许我们在组件安装时订阅，在组件卸载时取消订阅。理论上，如果路由器上下文发生变化，它将取消订阅旧的上下文，并重新订阅新的上下文(如果发生这种情况，这是一种理想的行为)，但没有理由假设这种情况会发生。

将我们的`/* TODO */`替换为以下内容:

```
useEffect(
  () => {
    // TODO: subscribe
    return () => {
      // TODO: unsubscribe
    };
  },
  [ /* TODO: memoization parameters here */ ]
); 
```

Enter fullscreen mode Exit fullscreen mode

`useEffect`采用一个将执行每次安装和更新的函数。如果该函数返回一个函数，那么第二个函数将执行每次卸载和预更新。

其中`effect1`是外部函数,`effect2`是内部函数，组件生命周期执行如下:

```
mount > effect1 ... effect2 > update > effect1 ... effect2 > unmount 
```

Enter fullscreen mode Exit fullscreen mode

外部函数在装载或更新后立即执行。内部函数*等待*,直到组件将要更新或卸载才执行。

我们的目标是在组件安装后订阅位置更改，并在组件卸载前取消订阅位置更改。

`useEffect`的记忆数组说“不要在更新时执行这些效果函数，除非这个参数数组已经改变。”我们可以用它来避免仅仅因为组件被重新渲染而持续订阅和取消订阅位置变化。只要路由器上下文是相同的，我们就不需要改变我们的订阅。因此，我们的 memoization 数组可以包含一个单项:`[ routerContext ]`。

```
useEffect(
  function subscribe() {
    // TODO
    return function unsubscribe() {
      // TODO
    };
  },
  [ routerContext ]
); 
```

Enter fullscreen mode Exit fullscreen mode

如何订阅位置更改？通过向`routerContext.history.listen`传递一个函数，该函数将在每次路由器历史改变时执行。在这种情况下，我们要执行的函数就是简单的`forceUpdate`。

```
useEffect(
  function subscribe() {
    routerContext.history.listen(forceUpdate);
    return unsubscribe() {
      // TODO
    };
  },
  [ routerContext ]
); 
```

Enter fullscreen mode Exit fullscreen mode

你如何退订位置变化？我们不能只让这个订阅在组件卸载后存在— `forceUpdate`会被调用，但是不会有组件更新！

`routerContext.history.listen`返回一个取消订阅函数，当调用该函数时，从事件中删除订阅监听器(`forceUpdate`)。

```
useEffect(
  () => {
    const unsubscribe = routerContext.history.listen(forceUpdate);
    return () => {
      unsubscribe();
    };
  },
  [ routerContext ]
); 
```

Enter fullscreen mode Exit fullscreen mode

并不是说这样做有什么好处，但是如果你想让这段代码更短一点，你可以:

```
useEffect(
  () => {
    const unsubscribe = routerContext.history.listen(forceUpdate);
    return unsubscribe;
  },
  [ routerContext ]
); 
```

Enter fullscreen mode Exit fullscreen mode

还有更短的:

```
useEffect(
  () => routerContext.history.listen(forceUpdate),
  [ routerContext ]
); 
```

Enter fullscreen mode Exit fullscreen mode

# 何去何从？🔮

由`react-router`包提供的`withRouter`的特定实现从组件属性中提取`history`、`location`和`match`，并赋予它们比上下文 API 值更高的优先级。这可能是因为`<Route>`组件提供了这些作为道具，而`match`的值需要来自`Route`的路径解释。

虽然我还没有在我的包中利用这一点，但我认为下一步应该是使用挂钩组件的道具作为`useReactRouter`的参数，允许它使用相同的道具优先级。

# 结论🔚

如果你想为这个开源包做贡献，或者在*打字稿*中看到它，你可以在 GitHub 上给它加星、分叉、打开问题，或者以其他方式[查看它。您可以通过 NPM](https://github.com/CharlesStover/use-react-router/blob/master/src/use-react-router.ts) 上的 [`use-react-router`自行使用该套餐。](https://www.npmjs.com/package/use-react-router)

如果你喜欢这篇文章，请随意给它一个心形或独角兽。又快又简单，还免费！如果你有任何问题或相关的好建议，请在下面的评论中留下。

要阅读更多我的专栏，你可以在 [LinkedIn](https://www.linkedin.com/in/charles-stover) 、 [Medium](https://medium.com/@Charles_Stover) 和 [Twitter](https://twitter.com/CharlesStover) 上关注我，或者[在 CharlesStover.com](https://charlesstover.com/)上查看我的作品集。