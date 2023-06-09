# drat 所有的📦！:借助您自己的 CRAN 式软件包回购程序，实现更轻松的软件包发现和安装

> 原文：<https://dev.to/hrbrmstr/drat-all-the---enabling-easier-package-discovery-and-installation-with-your-own-cran-like-repo-for-your-packages-4b22>

在[`CINC`](https://cinc.rud.is/)(“CINC 不是起重机”，听起来也像“同步”)我有一个正在进行中的[`drat`](http://eddelbuettel.github.io/drat/)起重机式回购(最终)我所有的包。与此同时，我的所有包都被放在同一个位置/迁移到 [SourceHut](https://git.sr.ht/~hrbrmstr) (只是等待 sr.ht alpha API 被烘焙)和一个[自托管的公共 Gitea 实例](https://git.rud.is/hrbrmstr)。一切仍将在你们使用的传统社交编码网站上，但最终目标是通过 CINC 库(即`install.packages()`)或通过这个独立的或任何社交编码网站的`remotes::install_git()`安装所有安装成为可能。

我最终会发布工作流程，但我的想法是在每个包回购中定制一个`pkgdown` YAML 文件，这样 navbar 就可以链接回 CINC 和其他页面(这将需要一些时间，因为这些年来我似乎已经制作了很多小包)，然后将一个包添加到 CINC 回购中:

*   从本地 Gitea 克隆它
*   构建源包
*   安装它
*   建立`pkgdown`站点，并让它转到`web/packages`目录(所以 https://cinc.rud.is/web/packages/hrbrthemes/对 https://cran.r-project.org/web/packages/hrbrthemes/)
*   使用`drat`将包添加到存储库中
*   自动更新有用的索引/包概述页面/目录

上述过程有助于我了解一些糟糕的 README 实践，以及如何(在未来)使安装 C[++]支持的包变得更容易。说到 READMEs，我还需要更新所有的 README，以便使用 CINC 的`install.packages()`或 Gitea 的`remotes`安装。

另外两个目标是可能添加二进制包版本(尽管这将是一个有趣的编排练习)，看看我是否能实现一些 [`notary`](https://github.com/ropenscilabs/notary) 概念。

这实际上是一个有趣的迷你项目，因为`drat`部分是简单的`drat::insertPackage('PKG', '/path/to/cinc')` ( *#ty Dirk！*)——尽管我需要思考一些关于维护存档版本和删除包的逻辑，这些`drat`还没有做，但也像删除 tarballs 和运行`tools::write_PACKAGES()`一样简单。

顺便说一句，我还`drat`-化了我们所有的$工作包，并通过静态 S3 虚拟主机使回购工作在内部可访问。托管对象的费用为每 GB 0.023 美元(每月)，每 1，000 个`GET`请求的费用为 0.0004 美元(加上 SSL 的最低设置费用)，非常便宜，而且非常易于维护。如果你对 S3 `drat`的更多细节感兴趣，请在评论中留言。

### 鳍

在几周的烘烤期后，Gitea 和 CINC 网站将禁用所有非错误的网络日志，错误日志不会保存 IP 地址或推荐人(我欢迎任何想要第三方审计`nginx`配置的人),因为另一个目标也是帮助人们不要成为科技创业公司或有着可怕邪恶历史的巨大、没有灵魂的全球跨国公司的产品。

在接下来的几周里，请留意完整的代码文章。

#### P.S

对于 10.14+版本的 Safari 用户，我对[站点](https://rud.is/)的“蝙蝠侠模式”版本做了一些调整。如果你确实使用 Safari ( *)但是…为什么？！*)并且对“黑暗模式”下的可读性有任何问题，请在评论中留言，我会尽力而为。