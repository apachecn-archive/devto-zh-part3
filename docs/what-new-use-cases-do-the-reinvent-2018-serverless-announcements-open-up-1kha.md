# re:Invent 2018 无服务器公告开辟了哪些新的使用案例？

> 原文：<https://dev.to/paulswail/what-new-use-cases-do-the-reinvent-2018-serverless-announcements-open-up-1kha>

2018 AWS re:Invent 大会刚刚结束。如果你像我一样，不幸没能去拉斯维加斯，你可能已经在推特上密切关注了 FOMO 的急性病例。

有许多令人兴奋的公告(完整列表[此处](https://serverless.com/blog/reinvent-2018-serverless-announcements/))，事实上，数量如此之多，以至于我感到有点不知所措，试图弄清楚这对于未来的云开发前景意味着什么。

因此，今天我花了几个小时浏览了大量的会议博客帖子，以期回答以下问题:

*   这种新东西适合什么类型的应用程序/工作负载？
*   我应该(以及如何)迁移我(或我的客户)现有的应用程序来使用这个新东西吗？
*   这一新事物是否让以前不准备使用无服务器的开发者或组织重新考虑？

让我们开始看主要公告...

## Lambda 现在通过“层”支持*任何*编程语言和可重用库

对于像 Ruby 和 PHP 这样的大型 web 开发社区来说，这是一个巨大的福音，当他们在云中运行他们的功能时，他们现在将被视为一等公民。虽然我不认为会有大规模的涌入(Rails 和 Laravel 之类的框架并不适合 Lambda)，但我确实希望看到许多开发人员尝试一些小的用例(例如构建一个 Lambda 网站联系表单)以及新的工作流和库/框架。

多语言支持通过一个名为**[λLayers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)**的新功能来实现。层也将为那些使用已经支持的语言的人带来好处。虽然以前功能是唯一的部署单元，但是现在层提供了依赖的工件，就像 ami 为 EC2 实例提供的方式一样。[官方博客文章](https://aws.amazon.com/blogs/aws/new-for-aws-lambda-use-any-programming-language-and-share-common-components/)称，你现在可以:

> *   Implement separation of concerns between dependencies and your custom business logic.
> *   Make your functional code smaller and more focused on what you want to build.
> *   Speed up deployment because there is less code to package and upload, and dependencies can be reused.

这看起来不错，因为[无服务器框架已经支持使用 Lambda 层](https://serverless.com/blog/publish-aws-lambda-layers-serverless-framework)💪，所以我希望很快就能在诸如从函数内部制作 FFmpeg 命令这样的事情上进行尝试。

我可以看到 Lambda 层被滥用的可能性，并期待在使用它们时会出现最佳实践。我不是唯一一个:

> ![Serverless / Green Data Advocate profile image](img/00a9e7dedba64031696aae84acaaf371.png)无服务器/绿色数据倡导者@ pauldjohnston![twitter logo](img/65e26e35707d96169ec8af6b3cbf2003.png)最佳 Lambda 层可能仍然遵循最佳实践...
> 
> 他们会很小，
> 做一件事*，
> 限制你的技术债务敞口
> 
> (还是要做尽职调查的人！)
> 
> 但是我好期待和他们一起玩
> 
> *“事”是相对的例如 NumPy/SciPy20:06PM-2018 年 11 月 30 日[![Twitter reply action](img/5d5a32424597af8488f7190c7d7d496b.png)](https://twitter.com/intent/tweet?in_reply_to=1068597099640643585)[![Twitter retweet action](img/3d12d4a909b79beaf8d81b6491fd052d.png)](https://twitter.com/intent/retweet?tweet_id=1068597099640643585)[![Twitter like action](img/3f89df2f36e8e5624d2a25952b3ac8b8.png)](https://twitter.com/intent/like?tweet_id=1068597099640643585)

## 通过应用负载均衡器从 EC2/containers 更轻松地迁移到 Lambda 架构

应用负载平衡器[现在支持 Lambda 函数作为它们的目标](https://aws.amazon.com/blogs/networking-and-content-delivery/lambda-functions-as-targets-for-application-load-balancers/)。
从表面上看，这似乎是一个奇怪的配对，因为 Lambda 函数通常是在基于 web API 的用例中从 API Gateway 调用的。在一个全新的无服务器项目中，你可能不会接近 ALBs。然而，如果你有一个现有的基于 EC2 的 web 应用程序运行在应用程序负载平衡器之后，那么迁移到 Lambda 可能是一个巨大的任务。

这项新功能允许您从现有的应用程序架构中一点一点地迁移出来。这种块可以小到单个端点上的单个路由上的单个动词。例如，您只需编写并部署一个新的 Lambda 函数来实现`GET /api/accounts/:account_id/products`路由，然后将其配置为从您现有的负载平衡器触发。你根本不用接触现有的生产应用程序代码。冲洗并重复剩余的 API 路径，直到您的服务器消失！

这是立即可用的，对通常的 ALB 和 Lambda 收费没有价格影响。我自己的 SaaS 产品仍然在 ALB 后面使用容器托管的 Express.js API 作为后台门户，我很久以前就想把它移植到 Lambda。有了这个声明，我现在有了一个开始分阶段迁移过程的低风险方法👍。

📖 ***相关:[如何计算将一个 EC2 app 迁移到λ](https://winterwindsoftware.com/ec2-to-serverless-migration-calculator/)***的计费节省

## λVPC 冷启动应该有所改善

我希望不止这些，但看起来 AWS 将在 2019 年发布一些东西，以帮助解决许多人的交易障碍——VPC 功能的缓慢冷启动时间:

液体错误:内部

📖 ***相关:[VPC](https://winterwindsoftware.com/scaling-lambdas-inside-vpc/)*T5】**

## 极光无服务器数据 API

受 VPC 冷启动影响的一个特殊用例是连接到 RDS 中的关系数据库。然而，随着 [Aurora 无服务器数据 API](https://aws.amazon.com/about-aws/whats-new/2018/11/aurora-serverless-data-api-beta/) 的发布，您现在可以通过 HTTP 而不是本地 SQL 协议调用您的数据库。这将有两个主要好处:

*   你不需要连接你的 Lambda 到 VPC，所以不会经历缓慢的冷启动
*   因为它只是一个 HTTP API，所以您不再需要担心 Lambda 函数(实际上是无状态的)遇到的 SQL 连接问题

这仍处于测试阶段，目前只支持 MySQL，所以你还不想在生产中使用它。
[杰瑞米·戴利](https://twitter.com/jeremy_daly/)贴出了一篇很棒的深度[如果你想进一步探索，先看看](https://www.jeremydaly.com/aurora-serverless-data-api-a-first-look/)的文章。他发现现在的性能很差(毕竟是测试版)，但希望随着服务的成熟，性能会有所改善。

## DynamoDB 点播

DynamoDB 一直被吹捧为 AWS 生态系统中无服务器应用程序的首选数据库。然而，我一直觉得它与许多人对无服务器的定义略有差距，因为你仍然需要做一些容量规划和设置自动扩展规则。我过去曾被供应不足的表的节流刺痛过。

然而，随着[按请求计费](https://aws.amazon.com/blogs/database/amazon-dynamodb-related-content-from-aws-reinvent-2018/)的宣布，DynamoDB 的扩展再次更加依赖于自动驾驶。在切换之前，您可能需要进行一些计算来规划未来的成本，但这更符合 Lambda 按使用量付费的计费模式，并减少了过度提供读写容量来处理繁忙时段的需求。

已经有一篇文章向你展示了如何使用无服务器框架实现这一点。

## 一种有效的选择

新的 [AWS Amplify 控制台](https://aws.amazon.com/about-aws/whats-new/2018/11/announcing-aws-amplify-console/)发布了，它似乎与 Netlify 在同一个空间:

> “AWS Amplify 控制台是一项持续部署和托管服务，适用于具有无服务器后端的现代 web 应用程序。现代网络应用包括 React、Angular 和 Vue 等单页面应用框架，以及 Jekyll、Hugo 和 Gatsby 等静态网站生成器。

我使用了上述 6 种技术中的 5 种，并且喜欢简单的自动化开发工作流程。我一定会去看看的。

## 其他需要关注的项目

我已经报道了我认为是主要的无服务器新闻，但还有一些其他公告值得一提:

*   现在[可以使用事务来更新 DynamoDB 表](https://aws.amazon.com/blogs/database/amazon-dynamodb-related-content-from-aws-reinvent-2018/)
*   发布了一个名为 [AWS Timestream](https://aws.amazon.com/timestream/) 的全新全托管时间序列数据库。可以看到这是有用的点击流分析。
*   API 网关现在支持 web 套接字:液体错误:内部

[AWS 前哨](https://aws.amazon.com/outposts/) ~~支持傻瓜运行自己的硬件~~ *“将本地 AWS 服务、基础设施和运营模式带到几乎任何数据中心、协同办公空间或内部设施。”*🤨

[AWS 鞭炮](https://aws.amazon.com/blogs/aws/firecracker-lightweight-virtualization-for-serverless-computing/)被宣布为*“无服务器计算的轻量级虚拟化”*。Twitter 上对此非常兴奋，但我有点“嗯”,因为它在堆栈中的位置太低，我不会考虑使用它。然而，如果拥有能够运行自己的 FaaS 执行环境的后备选项有助于降低保守组织中使用 Lambda 的锁定感，那么这可能是一件好事。

## 包装完毕

如果你想通过[完整列表](https://serverless.com/blog/reinvent-2018-serverless-announcements/)自己看看，还有许多其他新产品和功能我没有提到。

总而言之，这些公告坚定了我的信念，即无服务器是构建应用程序的未来，也是我推荐任何绿地项目开始的道路。现有应用程序的迁移路径使得切换到无服务器成为一个现实的选择。交易破坏者正在被解决，进入壁垒正在降低。

西蒙·沃德利的这条推文很好地总结了这一点:

> ![Simon Wardley #EEA profile image](img/677bce1fe8d308c602644d69fd9b6934.png)西蒙·沃德利# EEA@斯瓦德利![twitter logo](img/65e26e35707d96169ec8af6b3cbf2003.png)无服务器——感觉就像 2010 年那些令人兴奋的时光又在云端重现。赢家和输家正在你面前被创造出来，游戏将在一年左右结束，位置已经确定，未来正在展现。我们已经在最后的机会酒吧-玩家的最后赌注。2018 年 11 月 29 日 20:15 分[![Twitter reply action](img/5d5a32424597af8488f7190c7d7d496b.png)](https://twitter.com/intent/tweet?in_reply_to=1068237051584233472)[![Twitter retweet action](img/3d12d4a909b79beaf8d81b6491fd052d.png)](https://twitter.com/intent/retweet?tweet_id=1068237051584233472)[![Twitter like action](img/3f89df2f36e8e5624d2a25952b3ac8b8.png)](https://twitter.com/intent/like?tweet_id=1068237051584233472)

* * *

💌 ***如果你喜欢这篇文章，你可以注册[到我的关于在 AWS 中构建无服务器网络应用的每周时事通讯](https://winterwindsoftware.com/newsletter/)。***
你也可能会喜欢:

*   [担心无服务器带走](https://winterwindsoftware.com/concerns-that-serverless-takes-away/)
*   [生产中的 AWS 无服务器应用案例研究](https://winterwindsoftware.com/real-world-serverless-case-studies/)
*   [“无服务器”的不同定义](https://winterwindsoftware.com/serverless-definitions/)

*原载于 winterwindsoftware.com*。