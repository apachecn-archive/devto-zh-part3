# Ruby 面向对象编程简介

> 原文：<https://dev.to/nexttech/introduction-to-object-oriented-programming-with-ruby-46d6>

**面向对象编程** ( **OOP** )是一种围绕对象组织的编程范式。在高层次上，OOP 就是能够构建代码，以便其功能可以在整个应用程序中共享。如果做得好，OOP 可以产生非常优雅的程序，代码重复很少。

这与过程式编程(PP)相反，在过程式编程中，当您希望在应用程序的页面之间共享行为时，您可以按顺序构建程序并调用方法。常见的过程化编程语言有 [C](https://en.wikipedia.org/wiki/C_%28programming_language%29) 和 [Go](https://en.wikipedia.org/wiki/Go_%28programming_language%29) 。

在本教程中，你将学习到面向对象编程的基本概念，这是一种面向对象的编程语言，其中一切都是对象。我们将使用 Ruby，因为它的一个定义属性——除了优雅的语法和可读性——就是它如何实现 OOP 技术。这使得它成为开始学习 OOP 的绝佳语言。

我们将涵盖:

*   创建类
*   实例化对象
*   初始化参数
*   使用继承，以及
*   私有和公共方法。

在学习这些概念时，我们将构建自己的应用程序:一个 API 连接器，它与发送文本消息的应用程序进行动态通信。这将包括如何利用继承和对象实例化等概念，使我们的代码更具可伸缩性和可重用性。

#### 这个简短的教程改编自 Next Tech 的[Ruby 课程简介](https://c.next.tech/2EjW6vY)，其中包括一个浏览器内的沙盒环境和自动检查要完成的交互任务。

* * *

## 创建类

在我们开始之前，让我们定义一下什么是**对象**。本质上，对象是一段自包含的代码，包含数据(“属性”)和行为(“方法”)，并且可以与其他对象通信。相同类型的对象从**类**中创建，这些类充当定义属性和行为的蓝图。

用 Ruby 创建一个类相当容易。要定义一个类，只需键入`class`单词，后跟类名，并以`end`单词结束。包含在`class`和`end`之间的任何东西都属于这个类。

Ruby 中的类名有一个非常特殊的风格要求。它们需要以字母开头，如果它们代表多个单词，每个新单词也需要是大写字母，例如“CamelCase”。

我们将首先创建一个名为`ApiConnector` :
的类

```
class ApiConnector
end 
```

Ruby 中的类可以存储数据和方法。在许多传统的 OOP 语言(如 Java)中，您需要为每个想要包含在类中的数据元素创建两个方法。一种方法是 **setter** ，它设置类中的值。另一个方法是 **getter** ，它允许您检索值。

为每个数据属性创建 setter 和 getter 方法的过程可能会令人厌倦，并且会导致非常长的类定义。值得庆幸的是，Ruby 有一套叫做**属性访问器**的工具。

让我们为我们的类实现一些新的数据元素的 setters 和 getters。因为它是一个 API 连接器，所以拥有像`title`、`description`和`url`这样的数据元素是有意义的。我们可以用下面的代码添加这些元素:

```
class ApiConnector
  attr_accessor :title, :description, :url
end 
```

当你仅仅创建一个类时，它并不做任何事情——它只是一个定义。为了使用这个类，我们需要创建它的一个实例……我们将在下一步讨论它！

## 实例化

为了理解什么是**实例化**，让我们考虑一个现实世界的类比。让我们想象你正在建造一所房子。第一项任务是绘制房子的蓝图。这个蓝图将包含房子的属性和特征，例如每个房间的尺寸，管道如何流动，等等。

房子的蓝图是实际的房子吗？当然不是，它只是简单地列出了如何创建家庭的属性和设计元素。因此，在蓝图完成后，实际的家就可以建造了——或者说，“实例化”。

正如上一节所解释的，在 OOP 中，类是对象的蓝图。它简单地描述了一个对象的样子和行为。因此，实例化是获取类定义并创建可以在程序中使用的对象的过程。

让我们创建我们的`ApiConnector`类的一个新实例，并将其存储在一个名为`api` :
的变量中

```
api = ApiConnector.new 
```

现在我们已经创建了一个对象，我们可以使用`api`变量来处理类属性。例如，我们可以运行代码:

```
api.url = "https://next.tech/"
p api.url 
```

```
[Out:]
https://next.tech 
```

除了创建属性，你还可以在一个类中创建方法:

```
def test_method
  p "testing class call"
end 
```

要访问这个方法，我们可以使用与属性访问器相同的语法:

```
api.test_method 
```

综上所述，运行下面的完整类代码将导致打印出`url`和`test_method`消息:

```
class ApiConnector
  attr_accessor :title , :description , :url

  def test_method
    p  "testing class call"
  end
end

api =  ApiConnector.new

api.url = "https://next.tech/"
p api.url

api.test_method` 
```

```
[Out:]
"https://next.tech"
"testing class call" 
```

## 初始化器方法

在 Ruby 开发中，你可能会发现创建一个**初始化器**方法的能力很方便。这只是一个名为`initialize`的方法，每次创建类的实例时都会运行。在这个方法中，你可以给你的变量赋值，调用其他方法，并且做任何你认为在创建一个类的新实例时应该发生的事情。

让我们更新我们的`ApiConnector`来利用一个初始化器方法:

```
class ApiConnector
  def initialize(title, description, url)
    @title = title
    @description = description
    @url = url
  end
end 
```

在`initialize`方法中，我们为每个参数创建了一个实例变量，这样我们也可以在应用程序的其他部分使用这些变量。

我们还删除了`attr_accessor`方法，因为新的`initialize`方法将为我们处理这个问题。如果您需要在类外部调用数据元素的能力，那么您仍然需要有`attr_accessor`调用。

为了测试`initialize`方法是否有效，让我们在类中创建另一个方法来打印这些值:

```
def testing_initializer
  p @title
  p @description
  p @url
end 
```

最后，我们将实例化该类并测试初始化方法:

```
api = ApiConnector.new("My title", "My cool description", "https://next.tech")
api.testing_initializer 
```

```
[Out:]
"My title"
"My cool description"
"https://next.tech" 
```

### 用可选值工作

现在，当我们想让这些值中的一个可选时，会发生什么呢？例如，如果我们想给 URL 一个默认值呢？为此，我们可以用下面的语法更新我们的`initialize`方法:

```
def initialize(title, description, url = "https://next.tech") 
```

现在我们的程序将有相同的输出，即使我们在创建类的新实例时没有传递`url`值:

```
api = ApiConnector.new("My title", "My cool description") 
```

### 使用命名参数

虽然这看起来很简单，但是在真实的 Ruby 应用程序中传递参数可能会变得很复杂，因为有些方法可能需要大量的参数。在这种情况下，很难知道参数的顺序以及给它们分配什么值。

为了避免这种混淆，您可以使用命名参数，比如:

```
class ApiConnector
  def initialize(title:, description:, url: "https://next.tech")
    ...
  end
  ...
end

api = ApiConnector.new(title: "My title", description: "My cool description")
api.testing_initializer 
```

您可以在`initialize`方法中输入参数而不必查看顺序，甚至可以更改参数的顺序而不会导致错误:

```
api = ApiConnector.new(description: "My cool description", title: "My title") 
```

### 覆盖默认值

如果我们想覆盖一个默认值会发生什么？我们简单地像这样更新我们的实例化调用:

```
api = ApiConnector.new(title: "My title", description: "My cool description", url: "https://next.xyz") 
```

这次更新将覆盖我们的默认值`https://next.tech`，现在调用`api.testing_initializer`将打印`https://next.xyz`作为 URL。

## 继承

现在，我们将学习一个重要的面向对象原则，叫做**继承**。在深入了解它在 Ruby 中是如何执行的之前，让我们看看为什么它对构建应用程序很重要。

首先，继承意味着你的类可以有层次结构。当不同的类分担一些责任时，最好使用它，因为在每个类中为相同甚至相似的行为复制代码是一种不良的做法。

上我们的`ApiConnector`课。假设我们为各种平台提供了不同的 API 类，但是每个类都共享一些公共数据或流程。我们可以有一个共享数据和方法的父类**,而不是在每个 API 连接器类中复制代码。从那里，我们可以从这个父类创建**子类**。根据继承的工作方式，每个子类都可以访问父类提供的组件。**

 **例如，假设我们有三个 API:`SmsConnector`、`PhoneConnector`和`MailerConnector`。如果我们为这些类中的每一个单独编写代码，它看起来会像这样:

```
class SmsConnector
  def initialize(title:, description:, url: "https://next.tech")
    @title = title
    @description = description
    @url = url
  end

  def send_sms
    p "Sending SMS message with the title '#{@title}' and description '#{@description}'"
  end
end

class MailerConnector
  def initialize(title:, description:, url: "https://next.tech")
    @title = title
    @description = description
    @url = url
  end

  def send_mail
    p "Sending mail message with the title '#{@title}' and description '#{@description}'"
  end
end

class PhoneConnector
  def initialize(title:, description:, url: "https://next.tech")
    @title = title
    @description = description
    @url = url
  end

  def place_call
    p "Sending phone call with the title '#{@title}' and description '#{@description}'"
  end
end 
```

如您所见，我们只是在不同的类中重复相同的代码。这被认为是一种糟糕的编程实践，违反了开发的[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)(不要重复自己)原则。相反，我们可以创建一个`ApiConnector`父类，其他每个类都可以继承这个类的公共功能:

```
class ApiConnector
  def initialize(title:, description:, url: "https://next.tech")
    @title = title
    @description = description
    @url = url
  end
end

class SmsConnector < ApiConnector
  def send_sms
    p "Sending SMS message with the title '#{@title}' and description '#{@description}'"
  end
end

class MailerConnector < ApiConnector
  def send_mail
    p "Sending mail message with the title '#{@title}' and description '#{@description}'"
  end
end

class PhoneConnector < ApiConnector
  def place_call
    p "Sending phone call with the title '#{@title}' and description '#{@description}'"
  end
end 
```

通过利用继承，我们能够在整个类中删除所有重复的代码。

使用继承的语法是定义子类名，后跟`<`符号，然后是父类名——即我们的`SmsConnector`、`MailerConnector`和`PhoneConnector`类继承自`ApiConnector`类。

每个子类现在都可以访问父类`ApiConnector`中提供的全部元素。例如，如果我们用以下参数创建一个新的`SmsConnector`实例，我们可以调用`send_sms`方法:

```
sms = SmsConnector.new(title: "Hi there!", description: "I'm an SMS message")
sms.send_sms 
```

```
[Out:]
Sending SMS message with the title 'Hi there!' and description 'I'm an SMS message'. 
```

OOP 中的一个经验法则是确保一个类执行单一的职责。例如，`ApiConnector`类不应该发送短信、打电话或发电子邮件，因为这是三个核心职责。

## 私有和公有方法

在我们深入私有和公共方法之前，让我们首先回到我们最初的`ApiConnector`类，创建一个从`ApiConnector`继承的`SmsConnector`类。在这个类中，我们将创建一个名为`send_sms`的方法，它将运行一个联系 API 的脚本:

```
class ApiConnector
  def initialize(title:, url: 'https://next.tech')
    @title = title
    @url = url
  end
end

class SmsConnector < ApiConnector
  def send_sms
    `curl -X POST \
    -d "notification[title]=#{@title}" \
    -d "notification[url]=#{@url}" \
    "http://edutechional-smsy.herokuapp.com/notifications"`
  end
end 
```

这个短信 API 是 J. Hudgens (2017)创建的。

这个方法将发送一个`title`和`url`到一个 API，API 将依次发送一条 SMS 消息。现在我们可以实例化`SmsConnector`类并调用`send_sms`消息:

```
sms = SmsConnector.new(
  title: "Hey there!",
  url: "https://next.tech/xyz/introduction-to-ruby"
  )
sms.send_sms 
```

运行这段代码将联系 SMS API 并发送消息。你可以到[这一页](http://edutechional-smsy.herokuapp.com/notifications)的底部查看你的留言！

现在，使用这个例子，让我们讨论由类提供的方法的类型。

`send_sms`方法是一个**公共方法**。这意味着任何在我们的类上工作的人都可以用这个方法交流。如果你正在开发一个没有人在开发的应用程序，这可能看起来没什么大不了的。但是，如果您构建一个开源的 API 或代码库供其他人使用，那么您的公共方法表示您实际上希望其他开发人员使用的功能元素是至关重要的。

公共方法应该很少被改变。这是因为其他开发人员可能依赖您的公共方法来保持一致性，而对公共方法的更改可能会破坏他们程序的组件。

所以，如果你不能改变公共方法，你怎么能在一个生产应用程序上工作呢？这就是私有方法的用武之地。私有方法是一种只能由包含它的类访问的方法。它不应该被外部服务调用。这意味着您可以改变它们的行为，假设这些改变没有多米诺骨牌效应，并改变调用它们的公共方法。

通常私有方法放在文件的末尾，在所有公共方法之后。为了指定私有方法，我们在方法列表上面使用了`private`这个词。让我们给我们的`ApiConnector`类添加一个私有方法:

```
class ApiConnector
  def initialize(title:, url:)
    @title = title
    @url = url
    secret_method
  end

 private

   def secret_method
     p "A secret message from the parent class"
   end
end

api = ApiConnector.new(title: "My Title", url: "https://next.tech") 
```

注意我们是如何从`ApiConnector`类的`initialize`方法内部调用这个方法的？如果我们运行这段代码，它将给出以下输出:

```
[Out:]
A secret message from the parent class 
```

现在子类可以访问父类中的方法了，对吗？嗯，不总是这样。让我们从`ApiConnector`中的`initialize`方法中移除`secret_method`方法，并尝试从我们的`SmsConnector`子类中调用它，如下所示:

```
class ApiConnector
  def initialize(title:, url:)
    @title = title
    @url = url
  end

 private

   def secret_method
     p "A secret message from the parent class"
   end
end

class SmsConnector < ApiConnector
  def send_sms
    `curl -X POST \
    -d "notification[title]=#{@title}" \
    -d "notification[url]=#{@url}" \
    "http://edutechional-smsy.herokuapp.com/notifications"`
  end
end

sms = SmsConnector.new(
  title: "Hey there!",
  url: "https://next.tech/xyz/introduction-to-ruby"
  )
sms.secret_method 
```

```
[Out:]
Traceback (most recent call last):
main.rb:29:in `<main>': private method `secret_method' called for #SmsConnector:0x000056188cfe19b0> (NoMethodError) 
```

这是因为`SmsConnector`类只能访问父类的公共方法。私有方法本质上是私有的。这意味着它们只能由定义它们的类访问。

因此，一个好的经验法则是，当私有方法不应该在类外使用时，创建私有方法；当公共方法必须在整个应用程序中可用或由外部服务使用时，创建公共方法。

* * *

我希望你喜欢这篇关于 Ruby 面向对象编程基本概念的快速教程！我们讨论了创建类、属性访问器、实例化、初始化、继承以及私有和公共方法。

Ruby 是一种强大的面向对象语言，被流行的应用程序所使用，包括我们在 Next Tech 的应用程序。有了这些 OOP 的基础知识，你就可以开发自己的 Ruby 应用了！

* * *

如果你有兴趣学习更多关于 Ruby 编程的知识，请点击这里查看我们的 Ruby 入门课程[！在本课程中，我们涵盖了核心编程技能，如变量、字符串、循环和条件，更高级的 OOP 主题，以及错误处理。](https://c.next.tech/2EjW6vY)**