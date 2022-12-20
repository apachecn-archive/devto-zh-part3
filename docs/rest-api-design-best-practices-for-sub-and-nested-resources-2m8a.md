# 子资源和嵌套资源的 REST API 设计最佳实践

> 原文：<https://dev.to/moesif/rest-api-design-best-practices-for-sub-and-nested-resources-2m8a>

*[封面图片由 Marco Verch 专业摄影师兼演讲人，在 Flickr 上](https://www.flickr.com/photos/30478819@N08/)*

当我们开始设计一个 API 时，会出现许多问题，尤其是如果我们想要创建一个 REST API 并遵循 [REST 核心原则](https://www.moesif.com/blog/api-guide/getting-started-with-apis/#core-principles-of-restful-api):

*   客户机-服务器体系结构
*   无国籍
*   可缓存性
*   分层系统
*   统一界面

这个领域中经常争论的一个话题是资源的**嵌套，也称为**子资源**。**

*   为什么有人会窝他们的资源？
*   首先，它们是一个好主意吗？
*   我们应该把我们的资源藏起来吗？
*   我们应该什么时候嵌套我们的资源？
*   如果我们嵌套我们的资源，我们应该记住什么？

由于这个决定会对你的 API 的许多部分产生相当大的影响，比如安全性、可维护性或可变性，我想就这个主题进行一些阐述，希望它有助于做出更有教育意义的决定。

首先，我们将研究嵌套资源的原因。之后，我们将讨论导致嵌套资源出现问题的原因。

## 为什么

让我们从中心问题开始:**为什么要使用嵌套资源设计方法？**

我们为什么要使用这种方法:

```
/posts/:postId/comments/:commentId
/users/:userName/articles/:articleId 
```

Enter fullscreen mode Exit fullscreen mode

在这个问题上:

```
/comments/:commentId
/articles/:articleId 
```

Enter fullscreen mode Exit fullscreen mode

这种方法的主要原因是可读性；嵌套的资源 URL 可以传达一个资源属于另一个资源。它给出了层次关系的外观，就像文件系统中的目录一样。

这些 URL 传达的关系意义不大:

```
/books/:bookId
/rating/:ratingId 
```

Enter fullscreen mode Exit fullscreen mode

比这些网址:

```
/books/:bookId
/books/:bookId/ratings/:ratingId 
```

Enter fullscreen mode Exit fullscreen mode

我们可以直接看到我们请求的评级属于特定的书。在许多情况下，这可以使调试更容易。

我说*出现层次关系*是因为底层数据模型不一定是层次的。例如，在 GitHub 上，一个用户可以向多个存储库贡献代码，一个存储库可以有来自不同用户的贡献。这是一种多对多的关系。

```
/users/:userName/repos
/repos/:repoName/users 
```

Enter fullscreen mode Exit fullscreen mode

如果您只知道其中一个端点，这似乎是一个一对多的关系。

其他更技术性的原因是嵌套资源的*相对 id*或*上下文*。

例如，房子都有门牌号，但它们只限于它们所属的街道。如果你知道房子的门牌号码是 42，但你不记得街道，这对你没有多大帮助。

```
/street/:streetName/house/:houseNumber 
```

Enter fullscreen mode Exit fullscreen mode

另一个例子是文件系统中的文件名。如果在数百个不同的目录中有数百个这样命名的文件，仅仅知道我们的文件名为`README.md`是没有用的。

```
/home/kay/Development/project-a/README.md
/home/kay/Development/project-b/README.md 
```

Enter fullscreen mode Exit fullscreen mode

如果我们使用关系数据库，我们经常为所有的数据记录使用唯一的键，但是正如我们所看到的，对于其他类型的数据存储，比如文件系统，情况不一定如此。

嵌套的 URL 也很容易操作。如果一个层次结构被编码在一个 URL 中，我们可以删除 URL 的一部分来沿着这个层次结构向上爬。这使得具有嵌套资源的 API 的导航更加简单。

总而言之，我们希望使用嵌套资源**来提高可读性，从而提高开发人员的体验**，有时我们甚至**不得不**使用它们，因为**数据源没有给我们一种方法来仅仅通过 ID** 来识别嵌套资源。

## 为什么不

既然我们已经讨论了为什么我们应该使用嵌套的原因，那么讨论另一方面也很重要:**为什么我们不应该嵌套我们的资源呢？**

虽然筑巢有时是必要的，不可避免的，但它通常是一个我们应该记住的有特定成本或危险的选择。

让我们一个一个来看。

### 潜在的长网址

我们之前了解到嵌套资源可以使我们的 URL 更易读，但这不是一个确定的赌注。

特别是在资源之间有许多关系的相当复杂的系统中，嵌套方法会导致相当长且复杂的 URL。

```
/customers/:customerId/projects/:projectId/orders/:orderId/lines/:lineId 
```

Enter fullscreen mode Exit fullscreen mode

如果我们使用长字符串作为 id，这个问题会变得更加棘手:

```
/customers/8a007b15-1f39-45cd-afaf-fa6177ed1c3b/projects/b3f022a4-2970-4840-b9bb-3d14709c9d2a/orders/af6c1308-613f-40ff-9133-a6b993249c88/lines/3f16dca9-870e-4692-be2a-ea6d883b9dfd 
```

Enter fullscreen mode Exit fullscreen mode

因此，当我们开始沿着这条路走下去时，我们有时应该后退一步，看看我们是否仍在实现提高可读性的目标。

根据经验，最大嵌套深度是 2。有时候深度三也可以。例如，如果我们的 id 简短易读。

```
/author/kay-ploesser/book/react-from-zero/review/23 
```

Enter fullscreen mode Exit fullscreen mode

> _ 什么是 Moesif？ [Moesif](https://www.moesif.com/) 是最先进的 API 分析服务，已被 2000 多家组织用于了解您的客户如何使用您的 API 以及他们使用最多的资源。

### 冗余端点

一般来说，使用嵌套资源不如只使用根资源灵活。

例如，如果我们有一个多对多的关系。存储库有多个贡献者，但是每个用户也可以贡献给不同的存储库。

如果我们想用嵌套资源实现这一点，我们必须为这个关系单独创建两个端点

```
/user/:userName/repositories
/repositories/:repositoryName/contributors 
```

Enter fullscreen mode Exit fullscreen mode

如果我们想在没有嵌套的情况下实现这一点，我们可以为贡献定义一个根资源，它还允许在 URL 中使用过滤器参数。

```
/contributions?userName=:userName&repositoryName=:repositoryName 
```

Enter fullscreen mode Exit fullscreen mode

参数是可选的，所以我们也可以用它来获得所有的贡献，我们可以对它进行`PUT`和`POST`来改变和创建关系。

虽然这似乎不是一对多关系的问题，在一对多关系中，关系的一部分不能有多个连接，但我们仍然可以在某个点上希望搜索嵌套资源在其父资源中的所有记录。

所以当有了这个终点:

```
/mothers/:motherName/children 
```

Enter fullscreen mode Exit fullscreen mode

我们仍然希望得到所有母亲的所有孩子，并为此创建一个新的终点

```
/children 
```

Enter fullscreen mode Exit fullscreen mode

冗余的端点也增加了我们的 API 的表面，虽然我们的资源关系的更多可读的 URL 对于开发者体验是一件好事，但是大量的端点却不是。

多个端点增加了 API 所有者记录整个事情的工作量，并使新客户的加入更加麻烦。

返回相同表示的多个端点也可能导致缓存问题，并可能违反 RESTful API 设计的核心原则之一。

这个问题可以通过 HTTP 重定向来解决，所以所有的表示都从一个中央根资源返回，并且可以被缓存，但是仍然需要代码来实现它。

它还可能违反另一个核心原则，即[统一接口](https://www.moesif.com/blog/api-guide/getting-started-with-apis/#manipulation-of-resources-through-representations)。

> 当客户端拥有资源的表示(包括任何附加的元数据)时，它就有足够的信息来修改或删除服务器上的资源，前提是它有这样做的权限。

如果表示不包括关于嵌套的信息，并且我们没有根资源来直接访问它；我们不能创建、更新或删除它。

### 多个数据库查询

如果我们向下遍历关系图，而不是使用一个唯一的标识符(如果存在的话)从资源中检索一个表示，我们需要检查在 URL 中实现的关系是否成立。

以获取嵌套注释为例

```
/blogs/X/articles/Y/comments/Z 
```

Enter fullscreen mode Exit fullscreen mode

*   有 ID 为 X 的博客吗？
    *   我们去问问 DB 吧！
*   我们 ID 为 X 的博客有 ID 为 Y 的文章吗？
    *   我们去问问 DB 吧！
*   我们 ID 为 Y 的文章有 ID 为 Z 的评论吗？
    *   我们去问问 DB 吧！

获得所有博客的所有文章的所有评论也是一个问题。

1.  查询所有博客
2.  查询每个博客的每篇文章
3.  查询每篇文章的每条评论

这个 API 设计的 N+1 查询问题给了我们很大的打击。

如果我们的注释只有一个根资源，我们可以查询它，并在需要时加入一些过滤参数。如果注释有全局唯一的 id，我们可以直接查询它们。

```
/comments/Z
/comments?before=A&after=B 
```

Enter fullscreen mode Exit fullscreen mode

### 安全

如果我们共享资源的链接，URL 中编码的所有数据都有可能暴露给第三方，即使他们无权从我们的 API 请求表示。

当在互联网上通过 HTTP 请求任何东西时，中间用户会记录 URL，所以这些链接甚至不必在社交媒体等上主动共享。

例如，此图像链接:

```
/users/:userNaimg/:imageId 
```

Enter fullscreen mode Exit fullscreen mode

如果我们在某个地方分享它，我们这些人就会知道我们有一个特定名字的用户，他们在我们的服务上上传了图片。

如果图像链接是一个根资源，这样的信息是不明显的。

```
/images/:imageId 
```

Enter fullscreen mode Exit fullscreen mode

### 改变网址

如果我们的关系改变了，它们被编码到的网址就不再稳定了。

有时这可能是有用的，但更多的时候，我们希望保留我们的网址，这样旧的链接就不会停止工作。

例如，这种所有者-产品关系:

```
/owners/kay/products/1234
/owners/xing/products/1234 
```

Enter fullscreen mode Exit fullscreen mode

如果产品可以作为根资源访问，那么谁拥有它并不重要。

```
/products/1234 
```

Enter fullscreen mode Exit fullscreen mode

正如我之前提到的，如果关系经常变化，我们也可以考虑将关系本身视为一种资源。

```
/posessions?owner=kay&product=1234 
```

Enter fullscreen mode Exit fullscreen mode

使用这种方法，我们可以通过一个端点改变关系，但是通过不受这种改变影响的自己的根资源直接链接我们的其他资源。

## 总结起来

那么，这一切的要点是什么？

我们是否应该嵌套我们的资源？

有时这是无法避免的，因为数据源根本没有给我们任何其他选择，但如果我们有选择，我们应该考虑所有的利弊。

如果数据是严格的层次结构，不需要太多的嵌套，关系也不需要经常改变，我会选择嵌套资源。

对于开发者体验的胜利来说，不利因素并不太大。

如果数据容易发生关系变化，或者一开始就有非常复杂的关系，那么维护根资源甚至考虑完全不同的方法(如 GraphQL)会更容易。

更多的端点，正如嵌套场景所暗示的，更复杂的端点意味着要编写更多的代码和文档。这不会导致技能或知识方面的可行性问题，而通常只是开发和维护成本的问题。因此，即使我们知道如何做到这一点，并且安全性或可缓存性不是一个很大的问题，我们也必须问问自己，它是否给了我们任何竞争优势。

* * *

*Moesif 是最先进的 API 分析平台，支持 REST、GraphQL 等。超过 2000 个组织使用 Moesif 来跟踪他们最忠实的客户使用他们的 API 做了什么。* **[了解更多](https://www.moesif.com/features/?int_source=dev.to)**

* * *

最初发表于[www.moesif.com](http://www.moesif.com)