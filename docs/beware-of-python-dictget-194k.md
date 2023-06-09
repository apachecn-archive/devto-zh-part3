# 🐍小心 Python dict.get()

> 原文：<https://dev.to/daolf/beware-of-python-dictget-194k>

如果你认为`value = my_dict.get('my_key', 'default_value')`等同于
和`value = my_dict.get('my_key') or 'default_value'`,你可能应该读一下这个😃。如果你知道为什么不一样，那么你很可能在这里学不到东西。

## 好事:

任何使用 Python 3 的人都应该知道，dict API 非常清晰简单。我可以这样声明一个格言:

```
my_car = {'wheels': 4, 'brand': 'Tesla'} 
```

Enter fullscreen mode Exit fullscreen mode

这是简单、快捷和容易的。检索值很简单:

```
my_car.get('brand')
>'Tesla'
my_car['brand']
>'Tesla' 
```

Enter fullscreen mode Exit fullscreen mode

但是为了检索值，我更喜欢。get()有两个原因。首先，如果您想要访问的键不在这里，将不会出现异常(它将返回`None`)。第二，您可以向方法传递一个默认值，如果关键字不在字典中，该值将被返回:

```
my_car['color']
>KeyError: 'color'

my_car.get('color')
>

my_car.get('color', 'black')
>'black' 
```

Enter fullscreen mode Exit fullscreen mode

### 还有一个棘手的问题:

现在，我将向您展示在现实世界中发生了什么，同时在我编写的一个方法中修复了 [ShopToList](https://www.shoptolist.com) 的一个错误，该方法使用了一个[库](https://github.com/scrapinghub/extruct)从 HTML 页面(在本例中是一个电子商务页面)中提取元数据。

简而言之，我期望的数据应该是这样的(简化的例子):

```
data_from_extruct = {    
    'title': 't-shirt',    
    'brand': 'french-rocket',    
    'color': 'green',    
    'offer': {        
          'amount': 20,        
          'currency': '€'
    }
} 
```

Enter fullscreen mode Exit fullscreen mode

从这些数据中得到价格的最简单的方法是:

```
 price_from_extruct = data_from_extruct['offer']['amount']
    > 20 
```

Enter fullscreen mode Exit fullscreen mode

但是正如我之前所说，这个解决方案一点也不健壮。这是真实的世界，在真实的世界中，来自 extruct 的数据并不总是带有报价和报价中的价格。更好的方法是使用 dict.get:

```
price_from_extruct = data_from_extruct.get('offer').get('amount') 
```

Enter fullscreen mode Exit fullscreen mode

这仍然不够好，因为如果数据中没有报价，您将尝试执行第二个。对`None`执行 get('amount ')操作，这将引发一个错误。避免这种情况的方法是:

```
price_from_extruct = data_from_extruct.get('offer',{}).get('amount') 
```

Enter fullscreen mode Exit fullscreen mode

这里，如果我们在数据中没有 offer，第一个 get 将返回`{}`(空字典)而不是`None`，然后第二个 get 将执行一个空字典并返回`None`。一切都很好，看起来我们有一个健壮的方法来从格式不一致的数据中提取价格。当然，有时这个值会是 none，但至少这段代码不会中断。

好吧，我们错了。捕捉来自默认参数的行为。记住，当且仅当关键字**不在字典中**时，才会返回默认值。

它的意思是，如果你收到的数据是这样的:

```
data_from_extruct = {    
    'title': 't-shirt',    
    'brand': 'french-rocket',    
    'color': 'green',    
    'offer': None
} 
```

Enter fullscreen mode Exit fullscreen mode

那么前面的代码片段将会中断:

```
price_from_extruct = data_from_extruct.get('offer',{}).get('amount')
> AttributeError: 'NoneType' object has no attribute 'get' 
```

Enter fullscreen mode Exit fullscreen mode

这里没有返回 get 的默认值(' offer '，{})，因为关键的 offer 在 dict 中。它刚刚被设置为无。

当然 Python 很棒，所以有很多简单的方法可以解决这个问题。以下片段只是其中之一:

```
offers_from_extruct = data_from_extruct.get('offer') or {}
price_from_extruct = offers_from_extruct.get('amount') 
```

Enter fullscreen mode Exit fullscreen mode

当然，例如，如果 offer 的内容是一个列表，这也可以中断。但是为了举例，我们就讲到这里。

## 感谢阅读

希望这篇短文能帮你以后节省时间。我希望在本周花费大量时间试图修复某个 bug 之前知道这一点。

如果你喜欢这篇文章，请在评论中告诉我，不要忘记订阅我的时事通讯，还有更多(你还可以免费获得我下一本电子书的第一章)😎).

如果你喜欢 JS，我刚刚发表了[一些你可能会喜欢的东西](https://dev.to/daolf/things-you-should-know-about-js-events-4k2l)。

如果你喜欢 git，[我会帮你搞定](https://dev.to/daolf/git-series-13-understanding-git-for-real-by-exploring-the-git-director--5bd0)。