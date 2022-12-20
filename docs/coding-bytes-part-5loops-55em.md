# 编码字节:第 5 部分—循环

> 原文：<https://dev.to/waqardm/coding-bytes-part-5loops-55em>

# 什么是循环？

在编程中，`loops`用于根据设定的条件执行重复的任务。举个例子，如果我们想运行一段代码`x`的次数。

### 【for】循环

```
 // A random array with my items from my football kit
    const kit = ['Sweater', 'Shorts', 'Socks', 'Ball'];

    for (let i = 0; i < kit.length; i++) { 
        console.log(kit[i]);
    }

    /*
    Breakdown
    for (begin; condition; step) {
        // ... loop body ...
    }
    */ 
```

Enter fullscreen mode Exit fullscreen mode

for `loop`是最常用的，一开始可能很难理解是怎么回事，但是让我们来分解一下。首先，看看`syntax`，它就像一个`if`语句。您有`for`关键字，后面是条件的括号和将要循环的代码的花括号。

*   `const kit = ['Sweater', 'Shorts', 'Socks', 'Ball'];`
    我们将一个数组声明为`loop` over(这只是检查/通过的另一种说法)。

*   `for`与`if`相似，我们开始了`for loop`

*   这就是让人有点困惑的地方。对我来说，`i`是没有点击的部分。所以我们可以从这个开始。`i`可以是任何字母或单词，它的用法类似于变量，用来表示所讨论的元素。

*   看上面的例子，当我们声明`i = 0`时，我们在数组中的点`0`处请求`loop`到`begin`，这将是开始(毛衣)。(要了解为什么第一项是 0，你可以阅读这篇[文章](https://en.wikipedia.org/wiki/Zero-based_numbering))。

*   `i < kit.length`正在设置我们的`condition`状态，当`i`小于我们`kit`数组的`length`时，继续`looping`。

*   最后`i++`是每个`loop`要采取的步骤。在我们的例子中，在每个`loop`之后，我们希望`i`增加 1。

*   `{ console.log(kit[i]); }`
    在`loop`主体中，我们要求它在`loop`的每次迭代中对元素进行`console.log()`。

*   具体来说，`kit[i]`指的是数组的每个元素，其中`kit`是我们的数组，`[i]`指的是元素。

😬一开始可能有点疯狂，但是运行几次，然后试着输入示例代码，并观察控制台的输出。还有一个`for/in loop`我们以后会讲到，够了🤯暂时如此。

### 【while】循环

```
 let i = 0;
    while(i < 4){
        console.log(i)
        i++;
    }

    /* 
    Breakdown
    while (condition){
        code
        loop
    }
    */ 
```

Enter fullscreen mode Exit fullscreen mode

`Just be careful with ALL loops as you could end up running an endless loop if all the elements are not input correctly.`

用`while loop`你可以看到结构和语法上的相似之处。这些不太常见，但是一旦你理解了`for loop`，它就足够有意义了。😉

因为`loops`可能很难掌握，所以要尽可能多的练习。为什么不试试下面的任务呢？

### 进一步学习

```
 for (let i = 0; i < 10; i++) {
        console.log( i );
    } 
```

Enter fullscreen mode Exit fullscreen mode

1.  阅读上面的代码，并在自己编码之前写下您认为它将输出的内容。

2.  将中的`for loop`改为`while loop`。

如果你被困住了，给我发个微博😃。祝你好运，编码快乐！

这将是今年编码字节的最后一部分。对于那些庆祝的人，祝你们玩得开心，明年见。节日快乐！🎄

* * *

*感谢阅读。为了跟上我的编码之旅，来打个招呼吧👋在 [twitter](https://twitter.com/lawyerscode) 或我们的#devNewbie [Discord](https://discordapp.com/invite/mBsMP2H) 服务器上，我们有一群友好的学习者分享他们的经验。*