# JS 风格的 Lexing😎

> 原文：<https://dev.to/areknawo/lexing-in-js-style--b5e>

**这篇文章摘自[我的博客](https://areknawo.com)，所以请务必查看更多最新内容😉**

这篇文章是 AIM 项目系列文章的延续，所以如果你还没有阅读过,[以前的文章,](https://areknawo.com/tag/aim),我会给你一些答案。和*为什么？*提问。

在本文中，是时候真正开始编写 AIM 语言了！我将从创建一个 **lexer** 开始。一个**词法分析器**，或者如果你不喜欢酷名字- **记号赋予器**，是一个将人类可读文本转换成一系列**记号**的工具，用于以后的处理。它被用于创建编程语言，也用于文本处理和其他各种事情。所以，要注意的是，这不仅仅适用于创建编程语言。现在，看看这里的例子:

```
"128 + 428" 
```

Enter fullscreen mode Exit fullscreen mode

基本的，超级简单的两个数相加。现在让我们看看如何将它变成**令牌** :
的形式

```
["128", "+", "428"] 
```

Enter fullscreen mode Exit fullscreen mode

令牌不一定只有字符串。这些可以是例如包含附加的**元数据**的**对象**以供以后使用。本教程将展示如何实现一个基本的 lexer 来从上面的一种形式转换到另一种形式。

# 工装

自然，有很多图书馆和其他更大的创作来处理这类事情。最受欢迎的包括 **moo** 和 **lex** 。甚至有完整的工具包来帮助你创建**词法分析器和解析器**，例如 **nearley** 和 **jison** 。此外，对于该领域其他更专业的语言(如 C/C++)来说，这些列表可能会长得多，但这次是 JavaScript，或者更确切地说是 TypeScript。😀利用这些，你可以很容易很快地完成工作。但是，这不是本教程和整个 AIM 项目的目的，只是使用不同的库。不，这将从**开始自动执行**。现在-让我们开始吧！

# 定义

让我们首先定义我们的**词法分析器**应该是什么样子。
**应该是**:

*   以可移植和可扩展的形式实现所有的目标语法；
*   逐个标记地渐进扫描给定文本；
*   有迭代处理过的令牌的好方法；
*   提供令牌及其列表的基本版本的方法。

这是非常基本的东西——您应该从正确构建的 lexer 中期待的一切。接下来，我们需要决定如何准确地创建我们的 lexer。这种软件有 3 种标准解决方案:

*   通过使用多个正则表达式；
*   通过使用单个正则表达式；
*   通过逐字符阅读文本。

这里我们采用第二种选择。首先，利用正则表达式处理文本非常容易。它们让我们可以随时随地轻松扩展我们的语法。此外，当语法需要改变或发展时，逐字符阅读文本不是最佳解决方案。最后，对于第一个选项，单个 regexp 应该可以提供更好的性能。

# 咱们码！

我决定将代码分成 3 个基本文件:

*   *grammar.ts* -定义语法以备后用的文件，
*   *lexer . ts*——基础`Lexer`班的地方，
*   *token.ts -* 为`Token`阶层准备的场所。

## lexer.ts

我将从定义`Lexer`类及其方法开始:

```
import Token from "./token";

export interface GrammarStruct {
  id: string;
  match: string;
}

export default class Lexer {
  private index: number = 0;
  private expr: string = "";
  private regex?: RegExp;
  public tokens: Token[] = [];
  public column: number = 1;
  public line: number = 1;
  public data: string = "";
  public grammar: GrammarStruct[] = [
    {
      id: "newline",
      match: "\\n"
    },
    {
      id: "whitespace",
      match: "\\s"
    }
  ];

  private getRegex() {}
  public loadDefinition(def: GrammarStruct) {}
  public loadGrammar(grammar: GrammarStruct[]) {}
  public loadData(data: string) {}
  public next() {}
  public processAll() {}
  public update() {}
  public empty() {}
} 
```

Enter fullscreen mode Exit fullscreen mode

让我们进一步研究这个样板文件，并在后面介绍列出的方法的代码。

开头标记了一个导入的**标记**类和定义的`GrammarStruct`接口，用于指定单标记匹配的 regexp 容器应该是什么样子。接下来是`Lexer`类，它的属性很少，名字本身就说明了一切。其中 3 个被标记为**私有**，即`index`、`expr`和`regex`，因为这些是由 lexer 处理的，不应在它之外使用。现在，让我们继续讨论方法。

```
// ...
private getRegex() {
    if (!this.regex) {
      this.regex = new RegExp(this.expr, "gmu");
      console.log(this.regex);
    }
    this.regex.lastIndex = this.index;
    return this.regex;
  }
// ...

12345678910 
```

Enter fullscreen mode Exit fullscreen mode

第一个内部方法`getRegex()`用于从传递的`expr`(从连接的`GrammarStruct`匹配器生成)中生成单个 regexp，并确保当 regexp 需要重新生成时(当添加新的`GrammarStruct`)正确设置 lastIndex。

```
// ...
public loadDefinition(def: GrammarStruct) {
    if (this.expr.length > 0) this.expr += "|";
    this.expr += `(${def.match})`;
    this.regex = undefined;
    this.grammar.push(def);

    return this;
}

public loadGrammar(grammar: GrammarStruct[]) {
    for (const def of grammar) {
      this.loadDefinition(def);
    }

    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`loadDefinition()`和`loadGrammar()`函数负责加载`GrammarStruct` s，即将它们组合成一个匹配表达式。`loadDefinition()`加载单个`GrammarStruct`(匹配器定义)，而`loadGrammar()`加载它们的数组(整个语法)。`this`是为了更容易链接而返回的(也适用于其他方法)。

```
// ...
public loadData(data: string) {
    this.data += data;

    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

顾名思义——为 lexer 加载更多数据。数据只是一个字符串，附加在更长的字符串后面。

```
// ...
public next() {
    const regex = this.getRegex();
    const match = regex.exec(this.data);
    if (match) {
      const length = match[0].length;
      const token = this.grammar[match.indexOf(match[0], 1) - 1];
      const id = token.id;
      this.index += length;
      this.tokens.push(
        new Token(
          {
            column: this.column,
            line: this.line,
            value: match[0],
            length,
            id
          },
          this
        )
      );
      if (id === "newline") {
        this.column = 1;
        this.line++;
      } else if (id === "whitespace") {
        this.column++;
      } else {
        this.column += length;
      }

      return this.tokens[this.tokens.length - 1];
    }
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

比以前的任何一种方法都要复杂一些。但是这个也没有什么神奇的。它只是使用 regexp 匹配数据中的下一个令牌，对其进行处理，并根据生成的数据向列表中添加新的`Token`，即其**位置**、**长度**和 **ID** 。它还检查任何**换行符**和**空白符**(它们的匹配符在`Lexer`中默认预定义)，并正确处理它们以计算每个标记的位置(行号和列号)。

```
// ...
public processAll() {
    for (let i = 0; i < Infinity; i++) {
      const token = this.next();
      if (!token) break;
    }

    return this.tokens;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`processAll()`只是从`next()`方法派生出来的。基本上，它所做的就是匹配所提供数据中所有可能的标记，直到找不到标记，然后立即返回它们的完整列表。

```
// ...
public update() {
    this.tokens = this.tokens
      .filter(token => {
        return token.value && token.value !== "";
      })
      .sort((a, b) => {
        const line = a.line - b.line;
        const column = a.column - b.column;
        return line === 0 ? column : line;
      })
      .map((token, index, tokens) => {
        if (index > 0) {
          const previous = tokens[index - 1];
          if (previous.id === "newline") {
            return token.moveTo(previous.line + 1, 1, false);
          }
          return token.moveTo(
            previous.line,
            previous.column + previous.length,
            false
          );
        } else {
          return token.moveTo(1, 1, false);
        }
      });

    return this;
  }
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`update()`是游戏中的另一个大玩家。它以一种简洁、实用的方式对令牌数组进行排序和排列。首先，根据**为空** -没有值的令牌过滤数组。接下来，他们按照各自的位置进行排序。最后，对标记进行映射，使它们从第 1 行和第 1 列开始排列，这需要检查换行符和空格。这个方法在大多数的`Token`类方法中都有使用。

```
// ...
public empty() {
    this.data = "";
    this.line = 1;
    this.column = 1;
    this.index = 0;
    this.tokens = [];

    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`empty()`方法关闭列表。它会清空`Lexer`的数据以供重用(语法定义仍然被加载)。

`Lexer`课到此结束！这并没有那么复杂——如果真的复杂的话！但一切都应该是这样的——为什么要把这么容易解决的事情变成大问题呢？当然，可能会有一些改进，但基本思想是一样的。

## token.ts

在这个文件中，声明了更简单的`Token`类。基本上是这样的:

```
import Lexer from "./lexer";

interface TokenData {
  value: string;
  id: string;
  line: number;
  column: number;
  length: number;
}
export default class Token implements TokenData {
  public value: string;
  public id: string;
  public line: number;
  public column: number;
  public length: number;
  private lexer: Lexer;

  public constructor(params: TokenData, ctx: Lexer) {
    this.lexer = ctx;
    this.set(params, false);
  }
  public setValue(newValue: string, update = true) {}
  public moveTo(line?: number, column?: number, update = true) {}
  public moveBy(line?: number, column?: number, update = true) {}
  public set(params: Partial<TokenData>, update = true) {}
  public remove() {}
} 
```

Enter fullscreen mode Exit fullscreen mode

在最开始，我们有一个用于类型定义目的的`Lexer`类的导入和`TokenData`接口的声明，它定义了创建新令牌所需的所有值。`Token` class 只不过是一个简单的基本数据收集器，带有一些辅助函数。`Lexer`需要作为所谓的*上下文*来传递，以便其方法与`Token` API 进行后续交互。

```
// ...
public setValue(newValue: string, update = true) {
    this.value = newValue;
    this.length = newValue.length;
    if (update) {
      this.lexer.update();
    }
    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

做它应该做的事情——设置令牌的值和长度。这是许多标记编辑方法中的一种，可以选择用于生成标记的基本版本。它的第二个参数，默认值为`true`，指示`Lexer`是否应该在所有其他任务之后调用`update()`方法。

```
// ...
public moveTo(line?: number, column?: number, update = true) {
    line && (this.line = line);
    column && (this.column = column);
    if (update) {
      this.lexer.update();
    }
    return this;
}

public moveBy(line?: number, column?: number, update = true) {
    line && (this.line += line);
    column && (this.column += column);
    if (update) {
      this.lexer.update();
    }
    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`moveTo()`和`moveBy()`是用于重新定位已经匹配的令牌的实用方法。`moveTo()`将令牌移动到指定的行和列，`moveBy()`将令牌移动给定的行数和列数。移动被指示后，令牌通过`Lexer`的`update()`方法在数组中移动。

```
// ...
public set(params: Partial<TokenData>, update = true) {
    this.value = params.value || this.value;
    this.id = params.id || this.id;
    this.line = params.line || this.line;
    this.column = params.column || this.column;
    this.length = params.length || this.length;
    if (update) {
      this.lexer.update();
    }
    return this;
}
// ... 
```

Enter fullscreen mode Exit fullscreen mode

`set()`用于在一次调用中设置不同的令牌值。

```
// ...
public remove() {
    this.value = undefined;
    this.id = undefined;
    this.line = undefined;
    this.column = undefined;
    this.length = undefined;
    this.lexer.update();
 }
 // ... 
```

Enter fullscreen mode Exit fullscreen mode

`remove()`删除所有令牌的值，并运行`update()`方法，其中令牌由于缺少值而被从列表中过滤掉。

所以，`Token`类主要提供了一些编辑数据的方法。可能并不总是需要它，但它是一个很好的功能。

## [T1】grammar . ts](#grammarts)

```
import { GrammarStruct } from "./lexer";

const grammar: GrammarStruct[] = [
  // Comments
  {
    id: "single_line_comment_begin",
    match: ">>>"
  },
  {
    id: "multi_line_comment_begin",
    match: ">>"
  },
  {
    id: "multi_line_comment_end",
    match: "<<"
  }
  // ...
]

export default grammar; 
```

Enter fullscreen mode Exit fullscreen mode

在 *grammar.ts* 文件中，我们为对象数组定义了我们的语法。我们提供`id`作为匹配标记类型的标识符，并提供`match`作为字符串形式的 regexp，用于以后的连接。这里要注意一点。因为我们完整的正则表达式是以线性方式生成的，所以必须保持`GrammarStruct`匹配器的正确顺序。

# 海贼王

以上所有代码放在一起后(你可以在 [**AIM** multi-repo](https://github.com/areknawo/AIM) 的 **core** 包中找到完整的源代码)是时候使用这个创作了！这一切都归结为下面的代码:

```
import Lexer from "../src/lexer";
import grammar from "../src/grammar";

const AIMLexer = new Lexer().loadGrammar(grammar);
AIMLexer.loadData("public variable #int32 = 1")
AIMLexer.processAll() 
```

Enter fullscreen mode Exit fullscreen mode

现在，我可能会在这里结束这个故事，但还有一个问题。你看，lexer 只用于将线性文本处理成一个标记数组。这是另一个工具的工作- **解析器** -以正确的方式读取/处理它们。与这个问题特别相关的一个方面是我们语法中的**字符串**的实现。这主要是因为在 AIM 中创建类似 JS 模板文字的想法。如何用一个 regexp 匹配所有的可能性，例如转义值、字符和定位点？

```
"text\$${value}text"

1 
```

Enter fullscreen mode Exit fullscreen mode

简单的回答就是**你不**。也许解决方案对你们中的一些人来说是显而易见的，但它确实需要我进行一些深入的思考(很可能我不够开明)。你必须一个字符接一个字符地处理字符串**(至少这是我想出来的)。例如，看看我的语法定义数组的一部分。** 

```
[
    // ...
    {
        id: "char",
        match: `(?<=(?:(?:\\b|^)["'\`])|[\\x00-\\x7F])[\\x00-\\x7F](?=(?:[\\x00-\\x7F]+)?["'\`](?:\\b|$))`
    },
    // ...
    // Anchors and brackets
    {
        id: "string_anchor",
        match: "['`\"]"
    }
    // ...
] 
```

Enter fullscreen mode Exit fullscreen mode

我把字符串分成了锚和字符。这样，当我将它与任何给定的字符串进行匹配时，我会收到许多不同的标记，这些标记有`id` of **char** 和...那完全没问题！稍后我可以用解析器将它处理成最终的、好看的、快速的形式。

# 这只是开始

特别是与解析器和编译器相比，lexer 只是小菜一碟。但是把所有的谜题放在正确的地方真的很重要。只有基础牢固了，塔才不会倒。也就是说，我认为 lexer 的代码可能会发生一些变化(主要是在编写解析器的时候)，但主要思想将保持不变。

同样，如果你想看完整的代码，去 [**AIM** 回购](https://github.com/areknawo/AIM)。如果你想更密切地跟踪 AIM 开发的过程，可以考虑*开始回购*或者*在 [Twitter](https://twitter.com/areknawo) 上关注我。*💡