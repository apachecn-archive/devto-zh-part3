# 🌍开发人员的用户来自哪里？

> 原文：<https://dev.to/biros/where-are-dev-users-coming-from-4l53>

您可能已经知道，dev.to 有一个获取文章和用户的 API。

让我们用我最喜欢的 HTTP 客户端来试试吧， [httpie](https://httpie.org/) :

```
http https://dev.to/api/users/1 
```

Enter fullscreen mode Exit fullscreen mode

```
{  "github_username":  "benhalpern",  "id":  1,  "joined_at":  "Dec 27, 2015",  "location":  "Brooklyn, NY",  "name":  "Ben Halpern",  "profile_image":  "https://res.cloudinary.com/practicaldev/image/fetch/s--DOU9qJSH--/c_fill,f_auto,fl_progressive,h_320,q_auto,w_320/https://thepracticaldev.s3.amazonaws.com/uploads/user/profile_image/1/f451a206-11c8-4e3d-8936-143d0a7e65bb.png",  "summary":  "A Canadian software developer who thinks he’s funny.",  "twitter_username":  "bendhalpern",  "type_of":  "user",  "username":  "ben",  "website_url":  "http://benhalpern.com"  } 
```

Enter fullscreen mode Exit fullscreen mode

我首先注意到的是`id`似乎是自动递增的。
经过一些尝试，我很快发现 dev.to:上的用户总数大约在`119000`左右。
这给了我收集所有用户公开数据并尝试使用它们的想法。

让我们试试这个命令:

```
for i in {1..119000}; do http https://dev.to/api/users/$i >> users.dev.to.json && echo "," >> users.dev.to.json && echo "https://dev.to/api/users/$i"; done 
```

Enter fullscreen mode Exit fullscreen mode

在后台运行了几个小时后，我终于获得了我的 JSON 文件中的所有用户。

最后，我使用 [NoSQLBooster](https://nosqlbooster.com/home) 在 mongodb 集合中导入 JSON 文件。

用户档案中的信息不多，但有两条值得一看:`joined_at`和`location`。有一个图表显示注册随时间的进展可能会非常有趣。也许有一天我会这样做，但现在，让我们计算一些用户位置的统计数据。

我使用了两个查询来计算统计数据。这个帮助我得到了人们位置的大趋势:

```
db.getCollection("users.dev.to").aggregate(
    {"$group": {_id: "$location", count:{$sum:1}}}
).sort("-count") 
```

Enter fullscreen mode Exit fullscreen mode

然后，我使用另一个查询来获得每个位置的人数:

```
db.getCollection("users.dev.to").find({"location": /\bdublin\b/i}) 
```

Enter fullscreen mode Exit fullscreen mode

结论如下:

大多数用户(90%)没有填写他们的位置。
16.3%的填充位置的用户来自`USA`。
5.8%来自`India`。4.9%来自`UK`。
4%来自`Japan`。3.8%来自`Germany`。

让我们看看下面的完整统计数据:

| # | 国家 | 地区或城市 | #地区或城市 | #国家 | % |
| --- | --- | --- | --- | --- | --- |
| one | 美利坚合众国 |  |  | One thousand eight hundred and fifty | 16.32% |
|  |  | 包括纽约 | Two hundred and five |  |  |
|  |  | 包含 CA | One hundred and ninety-four |  |  |
|  |  | Incl NY | One hundred and forty-seven |  |  |
|  |  | 包括旧金山 | One hundred and thirty-seven |  |  |
|  |  | 包括发送 | One hundred and nineteen |  |  |
|  |  | 包括西雅图 | One hundred and seventeen |  |  |
|  |  | 包括洛杉矶 | One hundred and seven |  |  |
|  |  | 包括芝加哥 | One hundred and one |  |  |
|  |  | 包括华盛顿 | Eighty-one |  |  |
|  |  | 包含 FL | seventy-eight |  |  |
|  |  | 包括 PA | sixty-eight |  |  |
|  |  | 包含 MA | Sixty-seven |  |  |
|  |  | 包含 AZ | Thirty-eight |  |  |
|  |  | 包含 MI | Thirty-four |  |  |
|  |  | 包含哦 | Thirty |  |  |
| Two | 印度 |  |  | Six hundred and sixty | 5.82% |
|  |  | 包括班加罗尔 | One hundred and seven |  |  |
|  |  | 包括德里 | One hundred and three |  |  |
| three | 英国 |  |  | Five hundred and fifty-one | 4.86% |
|  |  | 包括伦敦 | Two hundred and fifty-eight |  |  |
|  |  | 包括英国 | Thirty-five |  |  |
|  |  | 包括苏格兰 | Thirty-three |  |  |
|  |  | 包括威尔士 | seven |  |  |
| four | 日本 |  |  | Four hundred and fifty-one | 3.98% |
|  |  | 包括东京 | Two hundred and forty-two |  |  |
| five | 德国 |  |  | Four hundred and twenty-eight | 3.77% |
|  |  | 包括柏林 | One hundred and twenty-eight |  |  |
| six | 巴西 |  |  | Three hundred and one | 2.65% |
| seven | 法国 |  |  | Two hundred and ninety-four | 2.59% |
|  |  | 包括巴黎 | Ninety-eight |  |  |
| eight | 加拿大 |  |  | Two hundred and fifty-seven | 2.27% |
|  |  | 包括多伦多 | One hundred and seven |  |  |
| nine | 尼日利亚 |  |  | One hundred and eighty-seven | 1.65% |
|  |  | 包括拉各斯 | Ninety-seven |  |  |
| Ten | 荷兰 |  |  | One hundred and fifty-four | 1.36% |
|  |  | 包括阿姆斯特丹 | Sixty-two |  |  |
| Eleven | 西班牙 |  |  | One hundred and ten | 0.97% |
| Twelve | 阿根廷 |  |  | One hundred and nine | 0.96% |
| Thirteen | 意大利 |  |  | One hundred and two | 0.90% |
| Fourteen | 印度尼西亚 |  |  | Ninety-two | 0.81% |
| Fifteen | 俄罗斯 |  |  | eighty-nine | 0.78% |
| Sixteen | 墨西哥 |  |  | Eighty-six | 0.76% |
| Seventeen | 菲律宾 |  |  | eighty-five | 0.75% |
| Eighteen | 波兰 |  |  | Eighty-one | 0.71% |
| Nineteen | 乌克兰 |  |  | Seventy-five | 0.66% |
| Twenty | 葡萄牙 |  |  | Sixty-three | 0.56% |
| Twenty-one | 南非 |  |  | Fifty-seven | 0.50% |
| Twenty-two | 巴基斯坦 |  |  | fifty-six | 0.49% |
| Twenty-three | 火鸡 |  |  | Fifty-five | 0.49% |
| Twenty-four | 比利时 |  |  | Fifty-three | 0.47% |
| Twenty-five | 埃及 |  |  | fifty-two | 0.46% |
| Twenty-six | 孟加拉国 |  |  | Fifty | 0.44% |
| Twenty-seven | 哥伦比亚 |  |  | Forty-six | 0.41% |
| Twenty-eight | 罗马尼亚 |  |  | Forty-five | 0.40% |
| Twenty-nine | 肯尼亚 |  |  | Forty-five | 0.40% |
| Thirty | 瑞士 |  |  | Forty-five | 0.40% |
| Thirty-one | 奥地利 |  |  | Forty-three | 0.38% |
| Thirty-two | 中国 |  |  | Forty | 0.35% |
| Thirty-three | 挪威 |  |  | Thirty-eight | 0.34% |
| Thirty-four | 爱尔兰 |  |  | Thirty-three | 0.29% |
|  | **主要地点总数**(如上所列) |  |  | **6683** | **58.94%** |
|  | **其他地点**(未列出) |  |  | **4656** | **41.06%** |
|  | **总位置** |  |  | **11339** | **100.00%** |
|  | *T2`<null>`* |  |  | *90260* | *80.80%* |
|  | *T2`<empty>`* |  |  | *10108* | *9.05%* |
|  | **总计** |  |  | **111707** | **100%** |

> 注:这些数字是一个月前的，但它们很好地概述了趋势。

感谢阅读！