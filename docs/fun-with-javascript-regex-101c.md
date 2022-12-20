# JavaScript 正则表达式的乐趣

> 原文：<https://dev.to/hexrcs/fun-with-javascript-regex-101c>

Regex，或者全称正则表达式，如果你不熟悉的话，会感觉像某种可怕的黑暗巫术。您知道这些神奇的咒语对于模式匹配和字符串解析来说非常强大，但是那些看起来怪异的问号、斜线和星号对于未经训练的人来说只是简单的胡言乱语。

并非所有的正则表达式都是相同的。我们今天在编程中使用的正则表达式有各种各样的语法风格。然而，现在最流行的大多是 Perl 的 regex 语法的衍生物。如果您已经掌握了一种正则表达式方言(就像我们今天要使用的 Javascript 方言，它与 Dart 的正则表达式语法 99%相同)，那么学习 Python 或 Java 之类的其他方言将是微不足道的。所以现在，让我们来点正则表达式的乐趣吧！

## 入门！

在 Javascript 中，“正则表达式模式”是一类对象，我们可以用关键字`new`或更简单的正则表达式文字(注意缺少引号)来初始化它。

```
const regex0 = new RegExp(',') // regex constructor
const regex1 = /,/ // regex literal 
```

Enter fullscreen mode Exit fullscreen mode

上面的两个`RegExp`对象是等价的——它们都代表单个逗号的“模式”。

那么现在我们已经定义了一个模式，我们如何使用它呢？如果我们关心的只是字符串中是否存在模式，我们可以简单地在一个`RegExp`对象上运行`test`方法。

```
const str0 = `1,000,000,000 is like, tres comas.`
console.log(regex1.test(str0)) // => true 
```

Enter fullscreen mode Exit fullscreen mode

如果我们想找到模式出现的位置，我们可以运行`exec`方法，比如对这个字符串执行正则表达式。

```
console.log(regex1.exec(str0))
// => [ ',', index: 1, input: '1,000,000,000 is like, tres comas.' ] 
```

Enter fullscreen mode Exit fullscreen mode

这是一些很酷的信息，但它只返回第一个匹配的索引。嗯，也许多次运行`exec()`可以达到目的，比如从迭代器中取出数据？

```
console.log(regex1.exec(str0))
// => [ ',', index: 1, input: '1,000,000,000 is like, tres comas.' ]
console.log(regex1.exec(str0))
// => [ ',', index: 1, input: '1,000,000,000 is like, tres comas.' ] 
```

Enter fullscreen mode Exit fullscreen mode

哎呀，没有！好吧，我们只是部分正确——`exec()`方法*确实是*有状态的，这个*是*遍历匹配的正确方式。问题实际上存在于我们定义的正则表达式模式中。

## 正则标志

标志允许我们切换应该如何执行搜索或匹配的选项，并且是正则表达式模式的一部分。

在最后一个例子中，我们需要的是一个*全局*标志`g`，它告诉正则表达式引擎进行“全局”搜索，而不仅仅停留在第一个匹配上(就像上面的例子)。`regex2`现在将在迭代完成时返回 null，然后从索引`0`重新开始。

```
const regex2 = /,/g
console.log(regex2.exec(str0))
// => [ ',', index: 1, input: '1,000,000,000 is like, tres comas.' ]
console.log(regex2.exec(str0))
// => [ ',', index: 5, input: '1,000,000,000 is like, tres comas.' ]
console.log(regex2.exec(str0))
// => [ ',', index: 9, input: '1,000,000,000 is like, tres comas.' ]
// let's only run 3 times for now 
```

Enter fullscreen mode Exit fullscreen mode

有一件有趣的事情需要观察——每个`RegExp`对象都有一个名为`lastIndex`的属性，使其有状态。然而，对象本身并不记得哪个字符串被传递给了`exec`方法。现在，我们的`regex2`对象将其`lastIndex`设置为`10`——如果我们将`str0`与另一个对象交换，匹配将从索引`10`而不是`0`开始。

```
console.log(regex2.lastIndex)
// => 10
const str1 = `This, is, cool.`
console.log(regex2.exec(str1))
// => null, because the searching starts at index 10. 
```

Enter fullscreen mode Exit fullscreen mode

其他有用的标志有:`i`使搜索不区分大小写，`m`基本上忽略换行符并进行多行搜索，以及其他较少使用的标志。今年 ECMAScript 2018 中添加了一个新的 *dotAll* `s`标志——这是一个非常有用的添加，因为点字符(`.`)现在终于匹配字符串中的所有*字符，包括`\n`换行符和 co。从 62 版开始，Chrome [就支持这个新标志。](https://www.chromestatus.com/feature/5644209772036096)*

现在让我们看看所有这些问号、斜线和星号到底是什么意思！

## 处理通配符

如果您熟悉 UNIX 或 Windows 风格的终端模拟器，您可能以前处理过通配符。你知道如何在 Mac 或 Linux 上使用`rm -f *.gif`来删除当前目录中的所有 gif 文件，不需要问任何问题，在你的 Windows 系统上使用`del *.gif /q`来做同样的事情。嗯，知道类 Perl 正则表达式中的通配符以其他方式工作是很重要的。

正则表达式中只有一个通配符——句点`.`(也就是点)。这个字符模式表示一个单独的未知字符，但不匹配换行符(`\n`)，所以`/c.t/`匹配字符串`cat`，不匹配`c\nt`。它基本上像您在命令行中熟悉的`?`通配符一样工作。

## 重复限定词(又名。量词)

那么如何匹配许多未知字符呢？这就是重复限定词的用武之地。

星号`*`代表 *0 个或更多*字符，`?`表示 *0 个或 1 个*字符，`+`表示 *1 个或更多*字符。

所以比如`essential`可以搭配`/es.*sential/`(此处为 *0* 多余字符)、`/es.+ential/`(此处为 *1* 多余字符)、`/es.?ential/` ( *1* 多余字符)，或者明显的`/.*/`。重复限定符也适用于特定字符，这允许`/ess?enstial/`匹配`essential`和`esential`，但不匹配任何其他字符串。

更重要的是，你可以用`{n,m}`来 DIY 重复的范围——最少 *n* 到最多*m*——或者用`{n}`来指定确切的发生次数。我们还可以用`{n,}`匹配 *n* 到无穷大(大于或等于 *n* )的出现次数。

例如，`essential`可以和`/es{2}ential/`匹配，`1000101`和`1000000101`都可以和`10{3,6}101`匹配，但是`10101`不能。

## 有时候我们需要逃避

有时，我们也需要匹配字符串中的字符，如`{`或`*`——我们可以使用反斜杠`\`来转义这些字符。在 JavaScript 中，要转义的特殊字符是`\ / [ { ( ? + * | . ^ $`。有趣的是，`] } )`并不是特殊人物，但是试图逃避它们并没有害处。您也可以对普通字符进行转义，但是您必须小心，因为在 regex 中，有一些字符类(比如针对所有数字字符的`\d`)写起来像转义符，但实际上不是——您可以将`/\o/`与`/dog/`匹配，但不能与`/\d/`匹配！

## 集合和类

当我们想要匹配特定集合中的字符时，字符类使我们的生活更容易。例如，如果我们想要匹配一个 ID 字符串中的数字，我们可以简单地使用`\d`来表示那个数字——本质上像一个点通配符，但只用于数字。

```
const regex = /\d+/g // the string must contain numbers
const str0 = '1234'
const str1 = 'D1234'
const str2 = 'D'

console.log(regex.test(str0)) // => true
console.log(regex.test(str1)) // => true
console.log(regex.test(str2)) // => false 
```

Enter fullscreen mode Exit fullscreen mode

我们也可以使用更灵活的集合符号`[0-9]`来代替`\d`——范围从 0 到 9。按照这个“范围”逻辑，对于基本的拉丁字母，我们也可以做`[a-z]`或`[A-Z]`，或者简单地做`[a-zA-Z]`。这些实际上只是为了简化`[0123456789]`或`[abcdef...]`而预先定义的快捷键。如果你要匹配扩展拉丁字母，你需要手动添加额外的字母。例如`[a-zA-ZüöäÜÖÄß]`为德语。你明白了吗😉。

您还可以在方括号内使用`^`作为求反运算符——它对方括号内的所有规则求反——`[^0-9]`将匹配除数字之外的所有内容。

需要注意的是，像`$`或`.`这样的特殊字符在括号内没有任何额外的含义——括号剥夺了它们所有的魔力，它们只是普通文本中可能出现的普通特殊字符。

## 预定义字符类简写

正如我们在上面看到的，Javascript regex(或任何其他 regex 语言)有一些针对常见情况的预定义简写。让我们看看下面的代码片段。

```
const regex1 = /\w/ // matches "word" characters - equivalent to [a-zA-Z0-9_]
const regex2 = /\s/ // matches "space" characters, also tabs and various unicode control code and stuff
const nregex1 = /\W/ // negated \w - matches everything other than "word" characters
const nregex2 = /\S/ // negated \s - matches everything other than "space" characters 
```

Enter fullscreen mode Exit fullscreen mode

## OR 运算符

像在普通编程语言中一样，`|`是*或*操作符。如果你喜欢尝试，也可以写成`[01234]|[56789]`！

## 替换成组

除了匹配模式，正则表达式对于替换匹配中的字符也非常有用。我们可以使用 JavaScript string 的`replace()`方法来做到这一点。

让我们首先构造一个电话号码匹配器。

```
const str0 = '+49 123-123-1234' // a phone number...
const regex0 = /^(\+\d+)\s(\d+)-(\d+)-(\d+)/g // matches the number and put all the digits into 4 groups.
regex0.test(str0); // => true, of course! 
```

Enter fullscreen mode Exit fullscreen mode

现在，如果我们使用`replace()`方法，我们可以使用`$`加上一个数字来表示我们在第二个(`replacer`)参数内的 regex 模式中定义的相应组。

例如，我们希望提取国家代码。

```
str0.replace(regex0, '$1') 
// replace the match (the whole string in this case) with the first matched group, which is  (\+\d+)
// => '+49' 
```

Enter fullscreen mode Exit fullscreen mode

或者用 *4321* 替换最后 4 个数字...

```
str0.replace(regex0, '$1 $2-$3-4321')
// => '+49 123-123-4321' 
```

Enter fullscreen mode Exit fullscreen mode

很有趣不是吗？😉

*最初在我的博客上发布[，在那里我每两周左右发布一些关于 web dev、flutter 和 ML 的东西。](https://www.xiaoru.li/post/fun-with-javascript-regex/)*

*你也可以在推特上找到我 [@hexrcs](https://twitter.com/hexrcs) :)*