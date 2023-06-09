# Javascript Array.push 比 Array.concat 快 945 倍🤯🤔

> 原文：<https://dev.to/uilicious/javascript-array-push-is-945x-faster-than-array-concat-1oki>

# TDLR

如果你要合并有数千个元素的数组，你可以通过使用`arr1.push(...arr2)`而不是`arr1 = arr1.concat(arr2)`来节省时间。如果你真的想走得更快，你甚至可能想写自己的实现来合并数组。

# 等一下...用`.concat`合并 15，000 个数组需要多长时间...

最近，我们有一个用户抱怨他们在 [UI-licious](https://uilicious.com) 上的 UI 测试执行速度明显变慢。每个通常需要 1 秒钟完成的`I.click` `I.fill` `I.see`命令(后处理，例如截图)现在需要 40 多秒才能完成，因此通常在 20 分钟内完成的测试套件反而需要几个小时，这严重限制了他们的部署流程。

我没花多长时间就设置了计时器来缩小导致速度变慢的代码部分，但是当我发现罪魁祸首时，我感到非常惊讶:

```
arr1 = arr1.concat(arr2) 
```

Enter fullscreen mode Exit fullscreen mode

数组的`.concat`方法。

为了允许使用简单的命令来编写测试，如`I.click("Login")`而不是 CSS 或 XPATH 选择器`I.click("#login-btn")`，UI-licious 使用动态代码分析来分析 DOM 树，以确定基于语义、可访问性属性和流行但非标准的模式测试什么以及如何测试您的网站。`.concat`操作被用来展平 DOM 树以进行分析，但是当 DOM 树非常大和非常深时效果很差，这发生在我们的用户最近向他们的应用程序推送更新时，这导致他们的页面显著膨胀(这是他们那边的另一个性能问题，但这是另一个话题)。

**用`.concat`合并 15，000 个平均大小为 5 个元素的数组花了 6 秒。**

**什么？**

6 秒...

对于平均大小为 5 个元素的 15，000 个数组？

那不是很多数据。

为什么这么慢？有没有更快的方法来合并数组？

* * *

# 基准比较

## [T1。推 vs。串联 10000 个数组，每个数组有 10 个元素](#push-vs-concat-for-10000-arrays-with-10-elements-each)

所以我开始研究(我指的是谷歌搜索)与 Javascript 中合并数组的其他方法相比的`.concat`基准。

原来合并数组最快的方法是使用接受 n 个参数的`.push`:

```
// Push contents of arr2 to arr1
arr1.push(arr2[0], arr2[1], arr2[3], ..., arr2[n])

// Since my arrays are not fixed in size, I used `apply` instead
Array.prototype.push.apply(arr1, arr2) 
```

Enter fullscreen mode Exit fullscreen mode

相比之下，它要快得多。

有多快？

我自己运行了几个性能基准来观察。你瞧，这就是 Chrome 的不同之处:

[![JsPerf - .push vs. .concat 10000 size-10 arrays (Chrome)](img/69b4bc7f01450ee1dfaff38173c6733f.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--I6TQ4Ugm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/txrl0qpb5oz46mqfy3zn.PNG)

👉[链接到 JsPerf 上的测试](https://jsperf.com/javascript-array-concat-vs-push/100)

要合并大小为 10 的数组 10，000 次，`.concat`以 0.40 ops/sec 的速度执行，而`.push`以 378 ops/sec 的速度执行。`push`比`concat`快 945 倍！这种差异可能不是线性的，但在这个小范围内已经很明显了。

在 Firefox 上，结果如下:

[![JsPerf - .push vs. .concat 10000 size-10 arrays (Firefox)](img/7700b38b7a325011d5d5c6e4ff83b473.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--1syE91oa--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/i8qyutk1h1azih06rn4z.PNG)

与 Chrome 的 V8 引擎相比，Firefox 的 SpiderMonkey Javascript 引擎通常较慢，但`.push`仍然名列前茅，速度快了 2260 倍。

对我们代码的这一个改变修复了整个减速问题。

## [T1。推 vs。串联两个数组，每个数组包含 50，000 个元素](#push-vs-concat-for-2-arrays-with-50000-elements-each)

但是，如果您不是合并 10，000 个大小为 10 的数组，而是合并 2 个各有 50000 个元素的巨型数组，会怎么样呢？

以下是 Chrome 上的结果和结果:

[![JsPerf - .push vs. .concat 2 size-50000 arrays (chrome)](img/febcc8b5b4aa18bf0d87b1033a9d76e0.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--pmnpnick--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/7euccbt97unwnjjdq5iw.PNG)

👉[链接到 JsPerf 上的测试](https://jsperf.com/javascript-array-concat-vs-push/170)

`.push`仍然比`.concat`快，但是快了 9 倍。

没有 945 倍慢，但仍然非常慢。

* * *

## 用 rest 展开的漂亮语法

如果您发现`Array.prototype.push.apply(arr1, arr2)`冗长，您可以使用一个简单的变体，使用 rest spread ES6 语法:

```
arr1.push(...arr2) 
```

Enter fullscreen mode Exit fullscreen mode

`Array.prototype.push.apply(arr1, arr2)`和`arr1.push(...arr2)`的性能差异可以忽略不计。

* * *

# 但是为什么`Array.concat`这么慢？

这很大程度上与 Javascript 引擎有关，但我不知道确切的答案，所以我问了我的朋友[@ pico creator](https://dev.to/picocreator),[GPU . js](http://gpu.rocks/)的联合创始人，因为他之前花了相当多的时间研究 V8 源代码。 [@picocreator](https://dev.to/picocreator) 还把他可爱的游戏 PC 借给了我，他用它来测试 GPU.js 以运行 JsPerf 测试，因为我的 MacBook 甚至没有内存来执行两个 50000 大小的阵列的`.concat`。

显然，答案与以下事实有很大关系:`.concat`创建一个新数组，而`.push`修改第一个数组。`.concat`将第一个数组中的元素添加到返回的数组中所做的额外工作是速度变慢的主要原因。

> 我:“什么？真的吗？就这样？但是差那么多？不会吧！”
> [@picocreator](https://dev.to/picocreator) :“正经的，就写点幼稚的实现试试。concat vs .push then！”

所以我试着写了一些`.concat`和`.push`的幼稚实现。事实上有几个，加上与[洛达什](https://lodash.com/)的`_.concat`的比较:

[![JsPerf - Various ways to merge arrays (Chrome)](img/412f0341faba5f716c235736a87bccb9.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--hIgqWvh5--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/w00r7grlnl1x5bnprrqy.PNG)

👉[链接到 JsPerf 上的测试](https://jsperf.com/merge-array-implementations/1)

## 天真实现 1

让我们谈谈第一套天真的实现:

##### 天真的实现`.concat`

```
// Create result array
var arr3 = []

// Add Array 1
for(var i = 0; i < arr1Length; i++){
  arr3[i] = arr1[i]
}

// Add Array 2
for(var i = 0; i < arr2Length; i++){
  arr3[arr1Length + i] = arr2[i]
} 
```

Enter fullscreen mode Exit fullscreen mode

##### 天真的实现`.push`

```
for(var i = 0; i < arr2Length; i++){
  arr1[arr1Length + i] = arr2[i]
} 
```

Enter fullscreen mode Exit fullscreen mode

正如你所看到的，两者之间唯一的区别是`.push`实现直接修改了第一个数组。

##### 香草方法的结果:

*   每秒 75 次运算
*   `.push`:每秒 793 次运算(快了 10 倍)

##### 天真实现的结果 1

*   每秒 536 次运算
*   `.push`:每秒 11，104 次运算(快 20 倍)

事实证明，我的 DIY `concat`和`push`比普通的实现要快...但是在这里我们可以看到，简单地创建一个新的结果数组并复制第一个数组的内容会大大减慢这个过程。

## 朴素实现 2(最终数组的预分配大小)

我们可以通过在添加元素之前预先分配数组的大小来进一步改进简单的实现，这带来了巨大的不同。

##### 天真的实现`.concat`与预分配

```
// Create result array with preallocated size
var arr3 = Array(arr1Length + arr2Length)

// Add Array 1
for(var i = 0; i < arr1Length; i++){
  arr3[i] = arr1[i]
}

// Add Array 2
for(var i = 0; i < arr2Length; i++){
  arr3[arr1Length + i] = arr2[i]
} 
```

Enter fullscreen mode Exit fullscreen mode

##### 天真的实现`.push`与预分配

```
// Pre allocate size
arr1.length = arr1Length + arr2Length

// Add arr2 items to arr1
for(var i = 0; i < arr2Length; i++){
  arr1[arr1Length + i] = arr2[i]
} 
```

Enter fullscreen mode Exit fullscreen mode

##### 天真实现的结果 1

*   每秒 536 次运算
*   `.push`:每秒 11，104 次运算(快 20 倍)

##### 天真实现的结果 2

*   每秒 1，578 次运算
*   `.push`:每秒 18，996 次运算(快 12 倍)

对于每种方法，预先分配最终数组的大小可以将性能提高 2-3 倍。

## `.push`数组与`.push`元素单独对比

好吧如果我们。单独推送元素？比`Array.prototype.push.apply(arr1, arr2)`和
快吗

```
for(var i = 0; i < arr2Length; i++){
  arr1.push(arr2[i])
} 
```

Enter fullscreen mode Exit fullscreen mode

##### 结果

*   整个阵列:每秒 793 次运算
*   单独元素:735 ops/sec(较慢)

因此，对单个元素执行`.push`比对整个数组执行`.push`要慢。有道理。

# 结论:为什么`.push`比`.concat`快

总之，`concat`比`.push`慢得多的主要原因是它创建了一个新数组，并做了额外的工作来复制第一个数组。

也就是说，现在对我来说还有另一个谜...

# 另一个谜

为什么普通实现比简单实现慢这么多？🤔再次向 [@picocreator](https://dev.to/picocreator) 求助。

我们看了一下 lodash 的`_.concat`实现，以获得一些关于 vanilla `.concat`在幕后还做了什么的提示，因为它在性能上是可比的(lodash 的稍快一些)。

事实证明，因为根据 vanilla 的`.concat`规范，该方法是重载的，并且支持两个签名:

1.  作为 n 个参数追加的值，例如`[1,2].concat(3,4,5)`
2.  要追加自身的数组，例如`[1,2].concat([3,4,5])`

你甚至可以这样做:`[1,2].concat(3,4,[5,6])`

Lodash 还处理这两种重载签名，为此，lodash 将所有参数放入一个数组中，并将其展平。如果你把几个数组作为参数传入是有意义的。但是当传递一个数组给 append 时，它不只是使用这个数组，而是将它复制到另一个数组中，然后将其展平。

...好的...

绝对可以更优化。这就是为什么您可能想自己动手实现合并数组。

此外，这只是我和 [@picocreator](https://dev.to/picocreator) 基于 Lodash 的源代码和他对 V8 源代码略显过时的知识对 vanilla `.concat`如何在引擎盖下工作的理论。

你可以在闲暇时阅读 lodash 的源代码[这里](https://github.com/lodash/lodash/blob/4.17.11/lodash.js#L6913)。

* * *

## 附加注释

1.  测试是用只包含整数的数组完成的。众所周知，Javascript 引擎处理类型化数组的速度更快。如果数组中有对象，结果会比较慢。

2.  下面是用于运行基准测试的 PC 的规格:
    [![PC specs for the performance tests](img/fc53baca0d0ee23bf98aca2b89a86b20.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--rsJtFcLH--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/fl7utbii6ivyifs66q2t.PNG)

* * *

# 为什么我们要在合法的测试中做如此大的数组操作？

[![Uilicious Snippet dev.to test](img/ddd4b0b6d1cbb7ddda303d99a1b421fa.png)](https://snippet.uilicious.com/test/public/1cUHCW368zsHrByzHCkzLE)

在幕后，UI-licious 测试引擎扫描目标应用程序的 DOM 树，评估语义、可访问属性和其他常见模式，以确定什么是目标元素以及如何测试它。

这是为了确保测试可以像这样简单地编写:

```
// Lets go to dev.to
I.goTo("https://dev.to")

// Fill up search
I.fill("Search", "uilicious")
I.pressEnter()

// I should see myself or my co-founder
I.see("Shi Ling")
I.see("Eugene Cheah") 
```

Enter fullscreen mode Exit fullscreen mode

不使用 CSS 或 XPATH 选择器，这样测试可读性更好，对 UI 的变化不太敏感，并且更容易维护。

### 注意:公益公告——请保持你的 DOM 低计数！

不幸的是，近来 DOM 树有变得过大的趋势，因为人们正在用现代前端框架构建越来越复杂和动态的应用程序。这是一把双刃剑，框架允许我们更快地开发，人们经常忘记框架增加了多少臃肿。在检查各种网站的源代码时，我有时会对那些只是用来包装其他元素的元素数量感到畏缩。

如果你想知道你的网站是否有太多的 DOM 节点，你可以运行一个 [Lighthouse](https://developers.google.com/web/tools/lighthouse/) 审计。

[![Google Lighthouse](img/4c9eea8f4b9ccc1661893f8146002ae3.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--OZ3aIjva--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://developers.google.com/web/progressive-web-apimg/pwa-lighthouse.png)

根据 Google 的说法，最佳的 DOM 树是:

*   少于 1500 个节点
*   小于 32 层的深度尺寸
*   父节点的子节点少于 60 个

对 Dev.to 提要的快速审计显示，DOM 树的大小相当不错:

*   总共 941 个节点
*   最大值 14 的深度
*   子元素的最大数量为 49

还不错！