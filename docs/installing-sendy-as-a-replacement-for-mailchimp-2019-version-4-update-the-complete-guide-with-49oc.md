# 安装熊智娟作为 Mailchimp-2019 版本 4 更新的替代品，完整指南，包含…

> 原文：<https://dev.to/lexjacobs/installing-sendy-as-a-replacement-for-mailchimp-2019-version-4-update-the-complete-guide-with-49oc>

### 安装熊智娟作为 Mailchimp-2019 版本 4 更新的替代品，完整指南，自动备份到 S3，以及 https 配置

#### 这些信息是从许多来源收集的，是我成功安装的完整记录。从这里开始可以节省自己很多时间。

### 您正在阅读的安装指南是针对熊智娟版本 4 的。它在熊智娟版本 4.0.2 上进行了测试，使用了数字海洋上可用的最新服务器配置

第 3 版安装指南[在我之前的文章](https://dev.to/lexjacobs/installing-sendy-as-a-replacement-for-mailchimp2019-update-the-complete-guide-with-automated-3359)中有概述。除了在 3.1.2.3 版本上测试之外，它几乎与本安装指南相同

### 变革的介绍和动机

我在 Mailchimp 上有一个新音乐发布邮件列表，用来发送不经常更新的内容。我从惨痛的教训中了解到，如果一个基本帐户两年没用，它就会被删除，旧的用户名会被永久屏蔽。刚刚蒸发了。删除前没有发送通知，也没有简单的方法来获取取消订阅的数据。Mailchimp 的合规部门称，这是一项“安全措施”。我仍然保留了原始订阅者邮件的备份，但没有这些年来之前取消订阅我的名单的重要记录。因此，虽然我可以很容易地用 Mailchimp 或另一个主机重新导入我的列表，但向过去取消订阅的人发送电子邮件会有“垃圾邮件发送者黑名单”的风险。

[![](img/7e31b26d2f6a571c42ecd2715d89408c.png)](https://cdn-images-1.medium.com/max/863/1*knqBax8wtKF8vVcupLhqGg.jpeg)

在经历了客户服务的重重困难后，我能够导出我以前所有的 Mailchimp 数据，但我确信找到行业巨头的替代产品更有意义，例如:Mailchimp、Sparkpost、ActiveCampaign、AWeber 或 Emma。

幸运的是，我最近浏览了韦斯·博斯(Wes Bos)的网络开发博客文章，想起了引人注目的标题“从 MailChimp 到熊智娟——我如何在我的电子邮件列表上每年节省 2400 美元”。由于我的电子邮件列表相对较小，这种变化不是为了省钱，而是为了保存我自己的列表，以避免另一个意外的帐户删除或其他与无法控制我自己的数据相关的问题。韦斯的文章说服我去看看熊智娟。这是一个自托管邮件列表软件解决方案。但是他的文章没有描述如何安装它。

购买后，通过产品支持论坛梳理，看起来 DigitalOcean 是一个基于价格和功能的主机托管的伟大选择。在阅读大量文档和堆栈溢出帖子时，

> 我在安装过程中做了详细的笔记，这样我就可以完整详细地记录下获得熊智娟运行实例的步骤。

我还将介绍如何使用免费的 Certbot 证书来保护熊智娟服务器与 T2 HTTPS 的流量，并以您选择的频率设置自动 mysql 数据库备份到 S3 的频率。

### 如何—从这里开始

*   [点击此处](https://sendy.co/?ref=vvD4p)阅读并向开发商购买熊智娟。
*   如果您还没有 DigitalOcean 的帐户，[通过此链接](https://m.do.co/c/c29fea8989aa)注册将获得 100 美元的免费信用点数，可使用 60 天以上。
*   你需要一个 AWS 账户。

既然熊智娟的开发商说“如果不成功，我们会退还你”，你可以尝试这整个实验，没有财务风险。我用每月 5 美元的标准液滴建立了我的熊智娟装置。唯一的其他费用是亚马逊 SES 电子邮件，每千封电子邮件**0.10 美元**，以及你在熊智娟购买软件的一次性费用**，在主要版本更新之前没有额外费用。**

 **#### 告诫

在这篇文章中，我再也不会用“简单”这个词了。但是我相信遵循这个指南会让你的安装过程尽可能的简单。

我建议先浏览一下，然后按照显示的顺序执行步骤。确保你有多余的头发以备不时之需。

> 我按照并验证了我在这里记录的所有说明，重新安装了熊智娟。

唯一不变的是变化，你可能会发现自己需要经历一些意想不到的困难，就像我在最初的安装过程中所做的那样。希望我记录的一切对你来说都是最新的和有用的。如果你发现有什么改变，请在评论中描述，我会相应地更新。

我特别确保实施一个符合现代标准的质量指南，确保使用一些冷笑话，至少一个科幻电影参考，偶尔插入表情符号。作为额外的奖励，我在整个指南中使用了示例域[www.contoso.com](http://www.contoso.com)。我还记录了实际的密钥/秘密，以及我在设置时使用的 ip 地址，这样您就不会有额外的心理负担去试图解析像。

> 我付出了额外的努力来描述你可能会遇到的各种场景，同时也描述了为什么要这样做，这样如果自从我记录了我的过程后某个步骤发生了变化，你就能够做出明智的选择。

**这是完全可行的**，所以请阅读本指南，热身你的谷歌印章，**我祝你安装过程顺利！**

每隔一段时间深呼吸、喝水、四处走走可能会有帮助。

### 文档约定

像这样的块中的文本表示您应该键入的内容或在浏览器页面或终端显示上可见的内容。

```
Text inside a box like this represents the terminal display

$ This represents your local terminal (after $)

# This represents the remote shell (after #) 
```

### 航行数字海洋:创建你的水滴(基于云的虚拟机)

登录到您的新帐户，并验证您有免费的 100 美元信用。如果您是新客户，但您没有通过推荐链接注册，请向客户服务发送一封带有推荐链接的电子邮件。根据我的经验，他们会信任你的。我在找到其他人的推荐链接后就注册了，当我把它用电子邮件发给他们时，他们给了我积分。如果你需要一个推荐链接，这里是我的:[https://m.do.co/c/c29fea8989aa](https://m.do.co/c/c29fea8989aa)

创建新的“项目”

[![](img/d955bdc640699fba23da8e3569305a9e.png)](https://cdn-images-1.medium.com/max/1024/1*D9a35tqzZqEbhA4Zxzh-ng.png)

下一个窗口将询问您:**将资源移动到新项目中？**点击**暂时跳过**

[![](img/512a853bfb36e82a3c78d7eb6d280999.png)](https://cdn-images-1.medium.com/max/1010/1*ynuytaca0iVyMD8A9WS4MQ.png)

接下来，创建你的 droplet，我将在下面详细说明设置:

[![](img/ac384cefd4fcfdcd2439e4e6ff02d6a9.png)](https://cdn-images-1.medium.com/max/536/1*Q86jeTB8V1ybHZy9XqvsEw.png)

单击蓝色按钮或绿色下拉菜单

[![](img/c5746b8fad8479e96a2712df410f7d95.png)](https://cdn-images-1.medium.com/max/628/1*eGpMmA8BWfLV859fdjy03w.png) 

<figcaption>点击水滴然后按照下面的说明进行设置</figcaption>

下一组选项是选择图像**。**选择市场

[![](img/84951e10faea287a22ca8242124bb946.png)](https://cdn-images-1.medium.com/max/1024/1*3VSjcXhWM8vk2xyBYWhMNQ.png) 

<figcaption>为了方便起见，我们打算在 Ubuntu 上安装一个预装的灯栈</figcaption>

在撰写本文时，LAMP Marketplace 应用程序的版本是 18.04。编号是 Ubuntu 的版本(linux 发行版的味道)。更新的版本(更高的数字)，如果可用，*可能会*呈现类似的安装体验。如果你有一些使用命令行的经验，并且不怕谷歌搜索，那就继续安装新版本吧。

[![](img/cfbe38831ae07b4bef9e27004552035a.png)](https://cdn-images-1.medium.com/max/738/1*7k8tRHKAVv_5GjINRWPN9w.png) 

<figcaption>撰写时最新发布</figcaption>

[![](img/60f19159bf415141c63277aec21ce90f.png)](https://cdn-images-1.medium.com/max/1024/1*bLVE0BbvCK4_J8Kfo_hwsQ.png) 

<figcaption>选择标准计划</figcaption>

[![](img/86e9e49cba93b0c73ffaedaa2843cb9b.png)](https://cdn-images-1.medium.com/max/104/1*Fbiab4msR040mKAqbDitGg.png) 

<figcaption>选择左箭头查看更小的液滴尺寸</figcaption>

[![](img/976b079b3452d97619f53d6a80539e30.png)](https://cdn-images-1.medium.com/max/372/1*4IaZyR9JWf9fpywsIuFxTg.png)

选择一个尺寸。对于我目前微不足道的邮件列表，我对标准计划和最小尺寸的虚拟机很有信心，我选择了 5 美元/月。将来，您可以根据自己的意愿扩展 cpu/ram。您可以扩大磁盘大小，但不能再次缩小。

**添加备份？**我选择不这样做，因为您可以制作和存储手动快照。但是，在一个每月 5 美元的容器上自动备份只会花费您每月 1 美元，因此您可以选择这样做，以便不必记住制作快照。

**添加块存储？**本教程不需要

**选择一个数据中心区域。**我选择了纽约。根据您存储的数据类型，您可能有在特定区域托管数据的法律义务，因此请做出相应的选择。

[![](img/b368363316306c09adf33be5fbfdca28.png)](https://cdn-images-1.medium.com/max/704/1*Dqt8lg6sfrdtGHuNCqx8Gg.png)

**选择附加选项？**我没有勾选所有这些选项

添加您的 SSH 密钥。我没有这样做，因此当我通过 SSH 登录时使用密码。初始 root 密码通过电子邮件发送。本教程讨论使用密码，所以现在跳过这一步。SSH 密钥允许您登录到远程服务器，而无需使用密码进行身份验证。

**完成并创建**。确保您只部署了 1 个 droplet，并将其重命名为 sane(如下面我的例子中的 sendy-droplet)，并确保它在您新创建的“项目”中(它将是，除非您已经有多个“项目”)

[![](img/80f1edc2472bf87785a6426cd9dea8b0.png)](https://cdn-images-1.medium.com/max/1024/1*jSBpKlGDqx6tY-534KCtvg.png)

点击“创建”按钮。然后你将被重定向到你的“项目”页面，并看到一个你的 droplet 创建进度的状态栏。需要几分钟。

[![](img/f307f50f9a34bbffa5b36b6e6d9d087a.png)](https://cdn-images-1.medium.com/max/1024/1*Wy6kD5gCQd68DcSdaG9m3w.png)

成功创建后，您的登录详细信息和密码已通过电子邮件发送给您。

[![](img/f879fbc515d1e7e1fb0939d03436b5ce.png)](https://cdn-images-1.medium.com/max/1024/1*EPhslJl3aKANZwfRPe-Z9w.png)

恭喜你，你的新创造正在云中运行。现在，通过在浏览器中输入 ip 地址来执行健全性检查，您应该会看到如下内容:

[![](img/794117148fb262b2447d8b51ef4c7c48.png)](https://cdn-images-1.medium.com/max/1024/1*bWamT-2YYLby1oJgLO6N3g.png)

“快速”启动指南按钮[指向数字海洋网站上的这个文档](https://www.digitalocean.com/docs/one-clicks/lamp/#start)，它详细说明了您的一键安装包含哪些软件，以及一些配置信息。

#### 让我们登录新的 droplet 并进行设置！

您将需要刚刚从 DigitalOcean 收到的电子邮件，其中包含您的 droplet 的 root 密码。我们将使用 **ssh** (安全外壳)登录。

```
$ ssh root@157.230.133.78 
```

迎接它的将会是以下适当的偏执信息

```
The authenticity of host '157.230.133.78 (157.230.133.78)' can't be established.
ECDSA key fingerprint is SHA256:0U9apoBN+7h1ajk2+KZoYQiL35X4Zjp+9MQLC71mX7y.
Are you sure you want to continue connecting (yes/no)? 
```

您确实想继续连接，所以键入 yes，然后您应该会看到以下内容:

```
Warning: Permanently added '157.230.133.78' (ECDSA) to the list of known hosts.

root@157.230.133.78's password: 
```

当您键入或粘贴密码时，终端不会显示您输入的字符。输入密码，然后按回车键，您应该登录并看到一些样板文本，描述您的服务器的一些默认配置。

系统会立即提示您重置 root 密码。首先输入您刚刚使用的当前密码。然后输入一个新的安全密码，你立即记录在某个安全的地方。你需要马上再次输入你的新密码来确认，然后你将登录到你的 droplet，一个 Linux Ubuntu 虚拟机。正常的 pwd ls cd mkdir etc 终端命令如您所预期的那样工作。

#### 更新可用软件列表

```
# apt-get update 
```

将更新可用软件包及其版本的列表，但不安装或升级任何软件包。它将创建如下所示的输出:

```
Get:1 [http://security.ubuntu.com/ubuntu](http://security.ubuntu.com/ubuntu) bionic-security InRelease [83.2 kB]
Hit:2 [http://ppa.launchpad.net/certbot/certbot/ubuntu](http://ppa.launchpad.net/certbot/certbot/ubuntu) bionic InRelease

\*\*\*[Lots more stuff …]\*\*\*

Get:37 http://mirrors.digitalocean.com/ubuntu bionic-backports/main Translation-en [448 B]
Get:38 http://mirrors.digitalocean.com/ubuntu bionic-backports/universe amd64 Packages [3468 B]
Fetched 14.0 MB in 5s (3035 kB/s)
Reading package lists... Done 
```

#### 升级可用包

# apt-get 升级

接受确认消息，您将被更新到所有已安装软件包的最新版本。

#### 安装一些熊智娟需要的新软件

接下来，我们需要安装几个软件包，它们不属于默认的一键式安装，但是熊智娟需要。第一个是 php-curl

#### 确定你需要哪一个版本的 curl

```
php --version 
```

将返回一系列输出，包括安装在服务器上的(major.minor.patch)版本。在我的例子中，我们可以在下一个命令中使用 7.2.10，但是我们只需要[major.minor]数字。

```
# apt-get install php7.2-curl 
```

如果不确定要安装哪个版本，请键入# apt-get install php7。然后按几次 tab 键，就可以看到您正在使用的 php 主要版本的各种软件包的自动完成。我的是用# apt-get install php7.2-curl 自动完成的，然后我点击 enter 安装，也接受了可能弹出的确认对话框。

下面是要安装的下一个:

```
# apt-get install php-xml 
```

接受可能弹出的确认对话框，然后使用以下命令重新启动 droplet 的 web 服务器:

```
# systemctl restart apache2 
```

#### 接下来，创建新的虚拟主机文件

> 虚拟主机文件是指定我们的虚拟主机的实际配置的文件，并指示 Apache web 服务器将如何响应各种域请求。

我们需要设置服务器来响应您在与您的域名注册商创建 DNS 记录时将要分配给它的域名。示例:如果您运行网站[www.contoso.com，](http://www.contoso.com,)并且您计划在 Sendy.contoso.com 安装熊智娟，那么您需要在 droplet 上配置 apache web 服务器，以便在您访问 sendy 域时提供适当的文件。这些文件位于/var/www/

#### 为要服务的文件创建目录结构

```
# mkdir -p /var/www/contoso.com/public\_html 
```

#### 创建新的虚拟主机文件

这些文件告诉 Apache web 服务器如何响应 web 请求。我们将复制并配置默认的主机文件(名为 000-default.conf)。以下 shell 命令是一行

```
# cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/contoso.com.conf 
```

现在我们将使用 nano 或 vim 编辑这个文件。如果您从未使用过 vim，我现在也不会开始。Nano 相当用户友好，响应箭头键移动在屏幕上移动光标，就像一个熟悉的文本编辑器。**编辑完成后，按** **控制键和** **x 键一起保存文件并返回终端。**

```
# nano /etc/apache2/sites-available/contoso.com.conf 
```

出现的文件应该以

```
<VirtualHost \*:80> 
```

结尾是

```
</VirtualHost> 
```

不要太担心中间有什么。它可能与您在下面看到的略有不同。

其他一切都保持原样。我们将在标签后添加一条指令(配置代码行):

```
ServerName sendy.contoso.com 
```

并修改两条指令(配置代码行)。首先，将文档根指令改为:

```
DocumentRoot /var/www/contoso.com/public\_html 
```

然后从如下所示的行中删除单词“Indexes ”:

```
Options Indexes FollowSymLinks 
```

因此，编辑后它看起来像这样:

```
Options FollowSymLinks 
```

然后还将标记内的文本修改为:

```
<Directory /var/www/contoso.com/public\_html/> 
```

最终结果应该类似于下面的内容。不要担心所有其他行完全匹配:

```
<VirtualHost \*:80>
        ServerName sendy.contoso.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/contoso.com/public\_html

        <Directory /var/www/contoso.com/public\_html/>
            Options FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE\_LOG\_DIR}/error.log
        CustomLog ${APACHE\_LOG\_DIR}/access.log combined

        <IfModule mod\_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

</VirtualHost> 
```

这是我的样子，但我正在安装熊智娟作为我的个人网站域的子域。如果您计划将熊智娟作为一个独立的服务器运行(类似于 [www.contoso.com)，](http://www.contoso.com)，)，那么您的最终结果应该是:(注意额外的指令 ServerAlias)

```
################# WHEN NOT INSTALLING AS A SUBDOMAIN

<VirtualHost \*:80>
        ServerName contoso.com
        ServerAlias www.contoso.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/contoso.com/public\_html

        <Directory /var/www/contoso.com/public\_html/>
            Options FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE\_LOG\_DIR}/error.log
        CustomLog ${APACHE\_LOG\_DIR}/access.log combined

        <IfModule mod\_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

</VirtualHost> 
```

如果你用 nano 编辑了，当你用 control + x 写并退出时，你会确认保存修改过的缓冲区吗？用 Y 然后再回车接受要写的文件名。

#### 激活虚拟主机文件(并停用默认主机文件)

```
# a2ensite contoso.com.conf 
```

这将显示一些确认消息，包括关于重启 apache 的通知。我们一会儿就去做。

首先，您需要使用以下命令停用默认虚拟主机文件:

```
# a2dissite 000-default.conf 
```

这也将显示一些确认消息。

正如确认消息所述，现在是时候重新加载 Apache 服务器以启用新的 Apache 配置了。

#### 重新加载 Apache 服务器

```
# systemctl reload apache2 
```

这只会重新加载 Apache web 服务器，您应该保持登录到 shell。

如果您现在访问该 ip 地址，您应该会看到如下内容:

[![](img/10e9efa68484cf4e90ebd330cfc0df6e.png)](https://cdn-images-1.medium.com/max/852/1*0hyK-SBJ57KhFMVwE5io3A.png)

这正是我们想要的。这是从标记内的 Options 指令中删除索引的结果。

**如果您看到类似这样的内容:**…

[![](img/8a69ccdba20a53f8b9ea2e45cc603771.png)](https://cdn-images-1.medium.com/max/676/1*4AApNkI--SlfhmnN8u5T1w.png)

…然后选项指令没有正确更新。检查上面的语法。更正之后，用# systemctl reload apache2 重新启动 Apache

> 当您重新加载浏览器时，它应该会显示正确的禁止消息。这将防止公众在你安装熊智娟后窥视你的/uploads 文件夹。

#### 关于 HTTPS 交通的点滴

由于一键式 droplet 预配置了 Certbot，因此只需几个命令就可以通过 SSL 保护您的服务器网络流量。然而，**在我们为服务器生成证书之前，我们需要设置到服务器的 DNS 路由，以确保我们为正确的域生成证书。**详细的 HTTPS 说明将在后面，首先我们将介绍如何为您的 droplet 配置正确的 DNS 记录。

### DNS 路由

登录您的域名注册商，以便我们可以创建一些指向您的新服务器的记录。

在我的例子中，对于这个示例服务器，我将为我的域名 contoso.com 的 DNS 记录添加一个新记录。我会创造一个 A 记录。A 名称记录应该是:sendy，值是:157.230.133.78

我刚刚用我的注册员测试了一下，几分钟后我的子域解析到了服务器，我看到了和直接访问 ip 地址时一样的图像。

如果您计划安装不带子域的独立服务器，那么您的“A”记录的名称将是 www，并且您将为裸根域(本例中为 contoso.com)创建第二个“A”记录。不同的注册商有不同的语法，可能是“@”，或者有时只是一个空白字段。这两个“A”记录的值就是您的 droplet 的 ip 地址。

理想情况下，传播您的 DNS 更改只需几分钟。感到不耐烦或怀疑它是否有效？看看这个(并用你自己的域名更新):

```
[https://www.whatsmydns.net/#A/sendy.contoso.com](https://www.whatsmydns.net/#A/sendy.contoso.com) 
```

这是一个非常酷的资源，它向您展示了 DNS 传播的状态。我的改变让它在几分钟内环游了世界。

### 用证书机器人启用 HTTPS SSP 证书安装

作为一键式安装的一部分，已经安装了一个名为 Certbot 的自动 SSL 证书生成器。现在我们已经正确设置了虚拟主机文件，并且正确配置了 DNS 记录，我们可以运行它了。

对于我的示例服务器，我只需要为 sendy 子域生成一个证书，下面是我将运行的内容:

```
# certbot --apache -d sendy.contoso.com 
```

如果您是为独立的熊智娟安装运行它，您将如下运行它(对于 www 子域和根域):

```
# certbot --apache -d contoso.com -d [www.contoso.com](http://www.contoso.com) 
```

运行该命令后，将会询问您

```
Enter email address (used for urgent renewal and security notices) (Enter 'c' to cancel): 
```

我个人认为输入一个您实际检查的电子邮件地址是一个非常好的主意，这样您就可以在自动 SSL 证书更新过程不起作用的情况下接收通知。如果您的证书过期，浏览以前的安全站点将不会显示您的站点，而是显示“您的连接不是私人的”警告页面。

如果您选择在没有电子邮件地址的情况下生成您的证书，您可以使用在提示符下按 c 键后给出的命令来完成。

在这一步之后，您将获得服务条款链接，并要求您同意。如果您同意，将会询问您是否要与 EFF 共享您的电子邮件地址。

回答这个问题后，您将看到一串与试图访问您的域的安装程序相关的输出。如果您的 DNS 传播尚未完成，或者您输入 DNS 记录的方式有错误，此步骤将失败。

到目前为止，输出应该是这样的:

```
root@sendy-droplet:~# certbot --apache -d sendy.contoso.com
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator apache, Installer apache
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for sendy.contoso.com
Enabled Apache rewrite module
Waiting for verification...
Cleaning up challenges
Created an SSL vhost at /etc/apache2/sites-available/contoso.com-le-ssl.conf
Enabled Apache socache\_shmcb module
Enabled Apache ssl module
Deploying Certificate to VirtualHost /etc/apache2/sites-available/contoso.com-le-ssl.conf
Enabling available site: /etc/apache2/sites-available/contoso.com-le-ssl.conf

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 
```

此时，系统会询问您是否也希望允许 HTTP 流量到达您的服务器，或者将其重定向到 HTTPS。我选择了 2，并让安装程序创建一个从 HTTP 到 HTTPS 的重定向。当需要配置熊智娟时，您可以指定是使用 HTTP 还是 HTTPS。您的熊智娟配置文件必须指定其中一个。

以下是剩余的输出:

```
Enabled Apache rewrite module
Redirecting vhost in /etc/apache2/sites-enabled/contoso.com.conf to ssl vhost in /etc/apache2/sites-available/contoso.com-le-ssl.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled
https://sendy.contoso.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=sendy.contoso.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/sendy.contoso.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/sendy.contoso.com/privkey.pem
   Your cert will expire on 2019-04-12\. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew \*all\* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

Donating to ISRG / Let's Encrypt: [https://letsencrypt.org/donate](https://letsencrypt.org/donate)
   Donating to EFF: [https://eff.org/donate-le](https://eff.org/donate-le) 
```

如果您想对这种工作方式感到更加安全，请继续并跟随测试站点的链接，该链接是在您应该在:短语之后输出的。对于这个网站将:[https://www.ssllabs.com/ssltest/analyze.html?d=sendy.contoso.com](https://www.ssllabs.com/ssltest/analyze.html?d=sendy.contoso.com)

现在，用浏览器刷新或导航到您的服务器 url(转到您的站点，如[https://www.contoso.com，](https://www.contoso.com,)不是 ip 地址)，您应该会看到:

[![](img/b611d0dbe23f5766bccba2032095a98a.png)](https://cdn-images-1.medium.com/max/172/1*8yQbWEAzfX2amtg6hxofwQ.png)T3】💥

您的安全服务器已启动并运行！

让我们来看看 ssl 站点的配置。conf 文件，并确保它正确地复制了原始非 ssl 版本中的指令。

```
# nano /etc/apache2/sites-available/contoso.com-le-ssl.conf 
```

应该会弹出一个类似这样的页面。查看 ServerName / DocumentRoot 和 Directory 指令看起来是否相似。另外，Options 目录没有 Indexes 关键字。如果这个。没有从 http 版本正确复制 conf 文件，您当前处于 nano 编辑器中，现在可以进行必要的更改。用 ctrl + x 退出编辑器，您将返回到 ssh 终端。

```
<IfModule mod\_ssl.c>
<VirtualHost \*:443>
        ServerName sendy.contoso.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/contoso.com/public\_html

        <Directory /var/www/contoso.com/public\_html/>
            Options FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE\_LOG\_DIR}/error.log
        CustomLog ${APACHE\_LOG\_DIR}/access.log combined

        <IfModule mod\_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

SSLCertificateFile /etc/letsencrypt/live/sendy.contoso.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/sendy.contoso.com/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule> 
```

接下来是 mysql 数据库的初始化，然后将熊智娟上传到你的服务器。

### 安装您的 Mysql 数据库

惊喜…已经装好了。欢迎再次享受一键安装的快乐。我们只需要创建熊智娟数据库，一个数据库用户/密码，并授予新数据库用户使用熊智娟数据库的特权。

使用# mysql 登录到 mysql，您应该会看到类似这样的内容:

```
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.26-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its affiliates. Other names may be trademarks of their respective owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

并创建您的熊智娟数据库(不要忘记；在末尾)

```
mysql> create database sendy; 
```

您应该会看到:

```
Query OK, 1 row affected (0.00 sec) 
```

现在我们需要创建一个带密码的数据库用户，并授予他们使用熊智娟数据库的权限(还记得；):

想出一个用户名和密码，并将其记录在安全的地方。你很快又会需要它的。

```
mysql> CREATE USER 'sendy\_db\_admin'@'localhost' IDENTIFIED BY '2zrQYe5NC8z2sw87TpC4SWDK'; 
```

它应该响应:

```
Query OK, 0 rows affected (0.00 sec) 
```

现在我们授予他们特权(记住；结尾):

```
mysql> GRANT ALL PRIVILEGES ON \*.\* TO 'sendy\_db\_admin'@'localhost'; 
```

同样，它应该响应:

```
Query OK, 0 rows affected (0.00 sec) 
```

我们的工作已经完成了。使用以下命令退出 mysql 命令行界面

```
mysql> exit; 
```

终端会说再见。现在，您又回到了 droplet 中提示 root 用户的地方。

现在已经有了数据库名、授权用户和密码，可以设置熊智娟配置文件了。

### 设置您的熊智娟配置文件

假设您已经购买并下载了熊智娟，解压文件并在代码编辑器中编辑/includes/config.php。

这是我们要修改的部分:

```
/\* Set the URL to your Sendy installation (without the trailing slash) \*/

define('APP\_PATH', 'http://your\_sendy\_installation\_url');

 /\* MySQL database connection credentials (please place values between the apostrophes) \*/

$dbHost = ''; //MySQL Hostname
 $dbUser = ''; //MySQL Username
 $dbPass = ''; //MySQL Password
 $dbName = ''; //MySQL Database Name 
```

本教程中安装的相关值为:

```
/\* Set the URL to your Sendy installation (without the trailing slash) \*/

define('APP\_PATH', 'https://sendy.contoso.com');

 /\* MySQL database connection credentials (please place values between the apostrophes) \*/

$dbHost = 'localhost'; //MySQL Hostname
$dbUser = 'sendy\_db\_admin'; //MySQL Username
$dbPass = '2zrQYe5NC8z2sw87TpC4SWDK'; //MySQL Password
$dbName = 'sendy'; //MySQL Database Name 
```

不管其他值如何，“localhost”都将是主机名。输入我们在上一步中刚刚创建的数据库用户的用户名/密码。并输入您在上一步中输入的数据库名称，在演示中是“sendy”。

注意:由于我们安装了 SSL 证书，因此“APP_PATH”值已被设置为 https，并且在。com

> 当你购买熊智娟的时候，你是否进入了一个不同于你将要使用的域名？没问题，只需在您的购买确认电子邮件中发送给您的“**更改您的许可域名”**链接中更新您的域名即可。我这样做过几次，更新很快。

现在是时候把熊智娟上传到你的服务器了。

### 上传熊智娟到你的服务器

您可以使用 sftp 或 rsync。Sftp 可能更直观，文档相对简单，所以我将展示 rsync 的命令。下面的命令将从一个新的终端窗口发出，而不是我们一直使用的 ssh shell。该命令应该作为一行输入。ahrvz 标志表示“归档/人类可读/递归/详细/压缩”。第一个文件夹路径是您的本地熊智娟文件夹(记得包括尾随/)，第二个位置是您的 droplet 中的 DocumentRoot web content 文件夹，这是我们在之前编辑的虚拟主机文件中指定的。发出以下命令后，系统会提示您提供 droplet root 密码:

**重要提示:以下命令必须在新的终端窗口中输入，而不是在我们一直用来向服务器发出命令的 SSH shell 中输入，(由命令开头的** **$指定)**

```
$ rsync -ahrvz -e ssh /path/to/your/sendy/folder/ root@157.230.133.78:/var/www/contoso.com/public\_html/ 
```

随后将进行确认，如:

```
root@157.230.133.78's password: 
```

在您成功输入 root 密码后，您将会看到一个巨大的文件列表，其中包含了将要传输到服务器的所有内容。文件的列出顺序可能会有所不同，但应该以类似如下的顺序开始:

```
root@157.230.133.78's password:
building file list ... done
./
.htaccess
\_compatibility.php
\_install.php
app.php
autoresponders-create.php
autoresponders-edit.php
autoresponders-emails.php 
```

以这样的话结尾:

```
locale/en\_US/
locale/en\_US/LC\_MESSAGES/
locale/en\_US/LC\_MESSAGES/default.mo
locale/en\_US/LC\_MESSAGES/default.po
uploads/

sent 5.62M bytes received 18.78K bytes 1.00M bytes/sec
total size is 13.41M speedup is 2.38

$ 
```

**重要提示:现在我们将再次向服务器发出 SSH shell 命令**

上传后，您需要确保上传文件夹的权限正确设置为 0777。这可以通过以下方式实现

```
# chmod 0777 /var/www/contoso.com/public\_html/uploads/ 
```

。你可以检查这是否有效

```
# ls -al /var/www/contoso.com/public\_html/uploads/ 
```

您应该会看到类似这样的内容

```
total 8
drwxrwxrwx 2 501 staff 4096 Jan 12 00:25 .
drwxr-xr-x 10 501 staff 4096 Jan 12 04:31 .. 
```

重要的权限位是与位于同一行的 drwxrwxrwx。

另一行与父文件夹的权限相关。

如果到目前为止一切都按计划进行，当您重新加载指向新服务器的浏览器时，您应该被重定向到/_install.php。

如果你看到以下内容，你正走在幸福的道路上:

[![](img/c7599dbbdc667286a4db769683c72107.png)](https://cdn-images-1.medium.com/max/1024/1*awTDewELFa5RTx1Byemuig.png)T3】💥

如果没有，也不用担心。您也可能会看到如下内容:

[![](img/79120ef4a2c7b36d9a0c0bac3d0aa4e7.png)](https://cdn-images-1.medium.com/max/1024/1*Ube6hCFODiaYytYoKu8K6Q.png)

不用担心，不管缺什么，都很容易改正。按照说明，我们导航到:/_compatibility.php？i=1，看看需要安装什么。你的结果可能略有不同，但如果你从一开始就遵循这个指南，可能不会太多。

[![](img/ffb72e39283fc3e80eea5aa0a0dfaa06.png)](https://cdn-images-1.medium.com/max/818/1*Qkr327UIbw64Y5aPPpgoaQ.png)T3】🤔

无论遗漏了什么，通过在搜索框中输入错误来搜索支持论坛:sendy.co/forum/，找出如何补救它。

在搜寻完任何可能的剩余软件包后，您的服务器兼容性清单应该都是绿色的，得分为 10/10，导航到您的熊智娟 url 应该会显示您的登录页面…

…或者可能是这样:

[![](img/866c0dd4f6a45786459d33f9aecdc96d.png)](https://cdn-images-1.medium.com/max/698/1*21Ax0plWOoaGJ-8xbuMWtg.png)T3】😶

如果这是您所看到的，您需要确保您在/includes/config.php 中作为 APP_PATH 变量输入的域与您当前使用浏览器指向的域相匹配，并且是您在购买产品时输入的域。域的改变可以很容易地完成。**该表格的链接在您购买软件**时收到的电子邮件中。在表单中，输入您的许可证密钥，并在“将域更改为”字段中输入更新的域。当您进入域时，不需要在它前面加上 http(s )(见下文)。然而，在熊智娟配置文件中定义 APP_PATHvariable 时，您确实需要 http(s)://preamble。

我需要在该表单中输入以下内容，以便熊智娟能够使用此处的示例:

[![](img/132a440048d62673da3dd9a0fd56294f.png)](https://cdn-images-1.medium.com/max/1024/1*vDgurnwxzJteeBkjnbqd0A.png)

对于熊智娟其余的设置，我把你留给官方入门文档的得力助手。你可以从第五步开始。关于如何正确设置您的 Amazon Web Services IAM 凭据，以及如何提高您的 SES 发送限额，有一些非常重要的信息。对于一个新帐户来说，这通常需要*至少* 2 天，因此，如果您正在浏览本文档并计划在不久的将来安装该软件:

> 我建议你现在就处理这个部分，这样当你准备好安装的时候，你就不需要等待 AWS 批准你增加的发送限制。

我将分享的一个相关提示是，当您第一次登录您的熊智娟仪表盘时，您可能会看到这个信息，**即使您已经提高了 SES 限额**:

[![](img/eab6ef67b83b5ae20e2fe56381a42c2f.png)](https://cdn-images-1.medium.com/max/302/1*4ZkuvzQ6-zi9dH0lixfiIg.png)

如果您知道您不再处于“沙盒模式”，解决方法是进入/settings 并确保设置正确:

[![](img/7af6089d5fb72134e9aef7ecf8d2f040.png)](https://cdn-images-1.medium.com/max/580/1*HTm1NqUS7cj5x58JvAp7dA.png) 

<figcaption>撰写本文时 3 个可用地区选择</figcaption>

那还剩下什么？**让我们设置数据库的自动备份，并将备份自动传输到 AWS S3 容器中！**

### 事情不顺利。通过设置自动备份和自动云存储这些备份，保护您宝贵的邮件列表数据。

#### 设置自动 Mysql 转储

在设置调度程序之前，让我们为备份和备份脚本创建一个存储目录。该命令:

```
# mkdir /srv/db\_bak/ 
```

将创建一个空文件夹，我们将把数据库备份转储到其中。

让我们再次使用 nano 编辑器创建实际的备份脚本:

```
# nano /srv/dump\_script.sh 
```

将下面的代码粘贴到编辑器中，按 Esc，然后按$将代码行自动换行，这样您就可以一次看到屏幕上的所有内容。对于第一个块，您将替换先前创建的数据库用户名和密码。第二行将 uploads 文件夹复制到备份目录中。最好有一个备份，因为你在创建活动时通过熊智娟上传的任何附件或图像都将存储在这个文件夹中，你发出的电子邮件将从你的 droplet 中提供。如果你要发送大量的电子邮件活动，你可能会想通过 CDN 或 S3 桶链接到图片和上传，而不会给你的服务器带来不必要的压力。

```
/usr/bin/mysqldump -u'sendy\_db\_admin' -p'2zrQYe5NC8z2sw87TpC4SWDK' sendy > /srv/db\_bak/$(
date +%F)\_sendy\_backup.sql

cp -r /var/www/contoso.com/public\_html/uploads/ /srv/db\_bak/ 
```

现在按 control + x 退出，并确认你要保存的修改，并确认文件名。现在来配置调度程序。

[![](img/3c75ac40f3d6e5400cc31bbc261b742f.png)](https://cdn-images-1.medium.com/max/572/1*5hZGqnUmGduzmkN0wISlTQ.png) 

<figcaption>不……克朗</figcaption>

有一个奇妙的实用程序存在于类 Unix 世界中，包括您的 droplet，称为 Cron。我们将使用它来创建两个职务。其中一个会定期给你的数据库拍快照。另一个将定期将这些快照转移到安全的云存储桶中。我将向您展示如何将这些备份转移到 AWS S3 存储桶。

最初，您的计算机上没有用于 cron 调度实用程序的用户配置文件，称为 crontab。下面是如何创建一个:

```
# crontab -e 
```

接下来可能是:

```
no crontab for root - using an empty one

Select an editor. To change later, run 'select-editor'.
  1\. /bin/nano <---- easiest
  2\. /usr/bin/vim.basic
  3\. /usr/bin/vim.tiny
  4\. /bin/ed

Choose 1-4 [1]: 
```

除非您已经知道基本的 vim 命令，否则请按 1 键选择默认的 nano 编辑器，然后按 enter 键。你将会看到这个屏幕，它显示了默认的文件，这个文件被“有用的”样板文件完全注释掉了

```
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '\*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 \* \* 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h dom mon dow command 
```

让我们删除样板文件，用以下内容替换它:

```
# backing up to /srv/db\_bak/
# every day at 8:12 utc, dump database
12 8 \* \* \* . /srv/dump\_script.sh > /dev/null 2>&1 
```

Crontable 调度语法的可读性并不好。如果你想了解这些模糊符号的更多细节，请访问这个网站。[https://crontab.guru/#12_8_*_*_*](https://crontab.guru/#12_8_*_*_*)

[![](img/deca11cd775fb2e5ed6bbfaef4ba0690.png)](https://cdn-images-1.medium.com/max/764/1*YzWjIvM9rA5appqDEAckbA.png)T3】点击带下划线的“下一步”展开显示

这将在每天 08:12 UTC 运行我们的数据库备份脚本，此时美国西海岸刚刚过午夜。我选择那个时间是因为不太可能会有很多人在那个时间以访问数据库的方式与我的站点进行交互，我希望它不要正好在 5 分钟的间隔，因为那是其他脚本运行的时间。也许没有必要这样安排，但我就是这么想的。

您可以随意将时间间隔设置为比每天更频繁，因为只要系统日期的当前日期相同，它就会覆盖以前的文件。它会将文件名保存为…

```
db\_bak/2019–05–03\_sendy\_backup.sql 
```

…随着日期的变化自动更新文件前缀。

我猜你可能想看到这实际工作，而不是等待一天。如果你想临时更新 cron 调度间隔**看看是否有效，将第三行的第一部分改为*/2 * * * *，它将每两分钟运行一次。也别忘了。在最后一个*之后。否则，转储脚本将无法运行。当你保存文件时，它会要求你确认保存，然后确认文件名。确认它向您显示的临时文件名，在您接受之后，它将安装一个新的 crontab，您可以使用 crontab -e 再次访问它进行编辑和更新。您还可以使用 crontab -l 随时检查您的 crontab。您还可以使用 select-editor 更改您首选的 crontab 编辑器。**

如果从 cron 脚本的末尾删除>/dev/null 2>&1 部分，那么作业的输出将在每次运行时生成一条系统邮件消息。可以用 cat /var/mail/root 检查，用 rm /var/mail/root 删除邮件消息。

由于在设置熊智娟后，您将很快回到 Crontab 文件中，因此我将为您节省一些时间，并为您提供将要设置的 cron 作业的代码，以便安排活动、激活自动响应程序以及导入面向订阅的 csv 文件。现在可以随意输入，或者等到您开始配置熊智娟仪表板时再输入。

```
# Five minute cron job to send scheduled campaigns
\*/5 \* \* \* \* php /var/www/contoso.com/public\_html/scheduled.php > /dev/null 2>&1

# One minute interval job for importing csv
\*/1 \* \* \* \* php /var/www/contoso.com/public\_html/import-csv.php > /dev/null 2>&1

# One minute interval job for activating auto responders
\*/1 \* \* \* \* php /var/www/contoso.com/public\_html/autoresponders.php > /dev/null 2>&1 
```

自动化任务的第 1 部分完成了！下一步是让这一切自动转移到你的 S3 桶。

#### 设置 Mysql 备份到 S3 存储桶的自动传输

首先，我们需要创建 AWS S3 存储桶，我们将把传输数据发送到这个存储桶中。从 S3 控制台，创建一个新的存储桶。区域是任意的，除非您有在特定区域托管数据的特定需求。

[![](img/77a333cd26e69ac1f01e8f949025923f.png)](https://cdn-images-1.medium.com/max/308/1*XyIHLGHhOmF4_RcEmelwNA.png)

[![](img/39408dccfda3c85cc5785cc83575b579.png)](https://cdn-images-1.medium.com/max/428/1*jjVBZS3KEduXbxhOS3Gmrg.png) 

<figcaption>名称和地区设置</figcaption>

然后单击下一步

[![](img/881663596513dcdfeeae93e94c971833.png)](https://cdn-images-1.medium.com/max/160/1*fSrH0qyEPr6JUuQoZGm0Ag.png)

我不需要在“配置选项”面板中做任何更改。

[![](img/f4e0b1aaceb4ab6b1bb48922f64a1483.png)](https://cdn-images-1.medium.com/max/197/1*BkukwiDOdcll9Sr5BNkdEA.png) 

<figcaption>再点击下一步</figcaption>

但是您需要适当地“设置权限”,以便能够上传到 bucket

[![](img/0d6b34779c0e8a081c1db3e88eaf3395.png)](https://cdn-images-1.medium.com/max/169/1*rEDWhKmLq_NceOjKObx8gQ.png)

[![](img/bbeab5f583243f17f5990d07e27efddb.png)](https://cdn-images-1.medium.com/max/1024/1*iG3zDtqgqvW0azDZZOqEWA.png) 

<figcaption>取消选中所有这些选项</figcaption>

[![](img/96d8393bbb610405b7c1a7983f1f4217.png)](https://cdn-images-1.medium.com/max/114/1*MeH7hagH2EUtdv2agpuXVg.png)

查看您的选项，然后按:

[![](img/9640f7b1207c22f321d96affb79426f1.png)](https://cdn-images-1.medium.com/max/276/1*W-eW52bG5Tw6bdPi_aTt9Q.png)

[![](img/fd5329b25e6fe29f7ff5b86f53df9a48.png)](https://cdn-images-1.medium.com/max/1024/1*FYtEgjGL237xW_HJ_Cq7DQ.png)

然后选择你的桶，选择**权限**，设置如下**访问控制列表:**

[![](img/bd4d810ef5aa624ed2142dcefd445d77.png)](https://cdn-images-1.medium.com/max/996/1*YayL7V4CJJkDOtocQFiD7A.png)

[![](img/57e16ec8d783a8a21436af5579300f79.png)](https://cdn-images-1.medium.com/max/340/1*NshnRDFD-YLVWMfUrBmxwQ.png)

[![](img/0615c7c5a5c9af1172ac4531402766bf.png)](https://cdn-images-1.medium.com/max/717/1*DqJllc3TsdICu1UHZJcHCw.png)

[![](img/f85ae72ce4e7ca7d4cb1fb9f966d9a08.png)](https://cdn-images-1.medium.com/max/520/1*ycP7kkYcZgMoCJgiRPilYw.png)

[![](img/c6bda519d3c118ff3bd16bc84bfbf4ac.png)](https://cdn-images-1.medium.com/max/1024/1*pSbdilPmxpMi-L9aXXwgnQ.png) 

<figcaption>【写对象】为所有人启用</figcaption>

然后选择“概述”选项卡并创建一个文件夹

[![](img/a63c1935c46c05dee65d9b92996c636e.png)](https://cdn-images-1.medium.com/max/1013/1*YeVeq86o1ASEx-U04y_sOg.png)

[![](img/7d72f3c96f10b824d1efe05aff44feab.png)](https://cdn-images-1.medium.com/max/160/1*xDgu2olMNPud0aC-KPHZ5g.png)

[![](img/74ecab7f969bc2c464837ae280ee17a2.png)](https://cdn-images-1.medium.com/max/1024/1*2iEvsIE7RjRi7Yd1h7cJwQ.png)

将存储桶和文件夹名称放在手边，您很快就会用到它们。

这就是设置铲斗的全部内容。现在创建一个用户，并给他们适当的权限在您的存储桶中存储东西。

#### 在 IAM 中设置 AWS 用户，并授予他们访问您的存储桶的权限

[![](img/006d9a859e05c9a74761f4ba0ce1c9ce.png)](https://cdn-images-1.medium.com/max/438/1*IwspZylIJst2zVHs6XT4UQ.png)

前往 AWS IAM[https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam/)/控制台，点击左侧的用户选项卡。点击添加用户，并给他们一个合理的名称和编程访问。然后单击下一步:权限。

[![](img/4c4cd1fc5c9a151bedb872732f460d7a.png)](https://cdn-images-1.medium.com/max/109/1*vDoRHMhiY19FIiTjdoQAHg.png)

[![](img/e753ee6e03b0eeed097a4bf1782a22a6.png)](https://cdn-images-1.medium.com/max/1024/1*3kjHx9aUjWjbLj-Eos2Bxw.png)

[![](img/ea3f25681b0dc5a9b992a57ac33b83ba.png)](https://cdn-images-1.medium.com/max/196/1*IKH48fBirgqktYDrI1oeZw.png)

直接选择附加的现有策略

[![](img/924dd80d3e38298cefe0764b64fc8740.png)](https://cdn-images-1.medium.com/max/804/1*fKMBZUPCtYrgyyBZxXs54Q.png)

在搜索框中输入:s3ReadOnly，并给用户 AmazonS3ReadOnlyAccess

[![](img/e24aa8e0c6bd0f0c4c860a43501c4eb9.png)](https://cdn-images-1.medium.com/max/840/1*pOu54eDfbu9NtJgzCb05oA.png)

然后点击下一步:标签。我没有在这里输入任何内容。接下来，单击“下一步:审查”,如果一切正常，单击“创建用户”

[![](img/d2fa4acf4d67ea16804bba7561d16154.png)](https://cdn-images-1.medium.com/max/127/1*btdKVLeHM8XdSBjtIo3IJg.png)

[![](img/bedf9205e367075fa4de5732763f848d.png)](https://cdn-images-1.medium.com/max/147/1*Is-OA-HnhgwZEHBJnhOrNw.png)

[![](img/e29404bcdd85080e84248e235eb9cae7.png)](https://cdn-images-1.medium.com/max/475/1*47ma5tdOVF99JX8GrjWeNA.png)

[![](img/c47c8fd81de209fca464bb02ecc6368f.png)](https://cdn-images-1.medium.com/max/133/1*D03p3vYFHxxCchQ9BLtdFg.png)

记下访问密钥 Id 和秘密访问密钥值。您很快就会需要这些值，这是您可以访问秘密访问密钥的唯一时间，因此如果您以后需要该值，您必须为该用户创建一个新的密钥。

[![](img/c532a3b3c8ab01fcf1dd1df7d93a6a86.png)](https://cdn-images-1.medium.com/max/902/1*VyDUok3zSRHN_SjAPN-d0g.png)

向终点冲刺！

#### 在您的 Droplet 上安装 AWS S3 实用程序，以处理您的数据库备份到 S3 存储桶的传输

回到您的根 SSH 终端连接，安装 S3 实用程序:

```
# apt-get install s3cmd 
```

接受安装确认，然后您将安装这些工具。现在您需要输入您的凭证。

```
# s3cmd --configure 
```

会弹出一个类似这样的对话:

```
Enter new values or accept defaults in brackets with Enter.
Refer to user manual for detailed description of all options.

Access key and Secret key are your identifiers for Amazon S3\. Leave them empty for using the env variables.
Access Key: 
```

您将输入您的访问密钥和秘密密钥，对于其余的值，只需单击 enter 接受缺省值，除非您想明确地设置一些内容。在过程的最后，确保当它测试你的凭证时，它通过，然后同意保存设置。

事情大概会是这样的:

```
Default Region [US]:

Use "s3.amazonaws.com" for S3 Endpoint and not modify it to the target Amazon S3.
S3 Endpoint [s3.amazonaws.com]:

Use "%(bucket)s.s3.amazonaws.com" to the target Amazon S3\. "%(bucket)s" and "%(location)s" vars can be used
if the target S3 system supports dns based buckets.
DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]:

Encryption password is used to protect your files from reading
by unauthorized persons while in transfer to S3
Encryption password:
Path to GPG program [/usr/bin/gpg]:

When using secure HTTPS protocol all communication with Amazon S3
servers is protected from 3rd party eavesdropping. This method is
slower than plain HTTP, and can only be proxied with Python 2.7 or newer
Use HTTPS protocol [Yes]:

On some networks all internet access must go through a HTTP proxy.
Try setting it here if you can't connect to S3 directly
HTTP Proxy server name:

New settings:
  Access Key: AKIAJ3O4AL4VMLGBEPAQ
  Secret Key: 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
  Default Region: US
  S3 Endpoint: s3.amazonaws.com
  DNS-style bucket+hostname:port template for accessing a bucket: %(bucket)s.s3.amazonaws.com
  Encryption password:
  Path to GPG program: /usr/bin/gpg
  Use HTTPS protocol: True
  HTTP Proxy server name:
  HTTP Proxy server port: 0

Test access with supplied credentials? [Y/n]
Please wait, attempting to list all buckets...
Success. Your access key and secret key worked fine :-)

Now verifying that encryption works...
Not configured. Never mind.

Save settings? [y/N] 
```

保存设置，这样就完成了。

假设您在那个文件夹中有一个 db 备份，那么您现在可以使用下面的命令来测试它。如果你还没有备份文件，先这样做:

# echo '只是一个测试文件'> /srv/db_bak/testFile.txt 然后:

```
s3cmd sync /srv/db\_bak/ s3://contoso-sendy-backups/sql-dumps/ 
```

这应该会产生如下所示的输出:

```
root@sendy-droplet:/srv# s3cmd sync /srv/db\_bak/ s3://contoso-sendy-backups/sql-dumps/
WARNING: Empty object name on S3 found, ignoring.
upload: '/srv/db\_bak/2019-01-12\_sendy\_backup.sql' -> 's3://contoso-sendy-backups/sql-dumps/2019-01-12\_sendy\_backup.sql' [1 of 1]
 7945 of 7945 100% in 0s 85.26 kB/s done
Done. Uploaded 7945 bytes in 1.0 seconds, 7.76 kB/s.
root@sendy-droplet:/srv# 
```

现在检查我的 S3 桶，我看到该文件夹的内容传输成功

[![](img/3fdc869485a24126a4dcd3536704546c.png)](https://cdn-images-1.medium.com/max/1024/1*5xU_uKKipomxWncc4vQ1mA.png)

好吧…让我们把这当成 Cron 的工作！

用 crontab -e 将它添加到您的 crontab 中

```
#every day at 9:12 utc, synchronize .sql dumps with s3 bucket
12 9 \* \* \* s3cmd sync /srv/db\_bak/ s3://contoso-sendy-backups/sql-dumps/ > /dev/null 2>&1 
```

这将在每天 9:12 UTC 将数据库转储发送到 S3。同样，您可能希望通过将时间间隔设置得更低来测试这一点(在上面的第一个 Cron 部分中有详细描述)，不要忘记将其设置回来。我认为每日外部云备份是一个合理的默认设置。您的需求可能不同。

### 从保存的备份中恢复数据库

那么，如果发生数据库灾难，您会怎么做呢？如果您如上所述设置了 S3 自动备份，您就有了一个备份！

可以通过以下方式恢复数据库:

*   登录您的 S3 控制台
*   选择要恢复的备份

[![](img/352fdc03d38feea80cf6e9694a8eaf79.png)](https://cdn-images-1.medium.com/max/318/1*f-SrhKJFVZ0egLpn5kit0Q.png)

*   当您下载该文件时，它可能会被下载为. txt 文件。只需将文件扩展名重命名为。结构化查询语言
*   从命令行将备份发送到您的 droplet，使用(这都是一行):

```
$ rsync -ahrvz -e ssh /path/to/your/sql/backup/2019-01-14\_full\_sendy.sql root@157.230.133.78:/srv/db\_bak/backup\_to\_restore.sql 
```

现在，登录到您的 droplet，然后登录到 mysql，删除现有的损坏的数据库。*** *这将不可逆转地破坏当前数据库**** 因此，请确保仅在数据库已经损坏并且需要从您的某个备份中恢复时才执行此操作。

```
# mysql 
```

然后(**这是破坏性部分**):

```
mysql> drop database sendy; 
```

然后重新创建 sendy 数据库

```
mysql> create database sendy; 
```

然后使用以下命令退出我的 sql:

```
mysql> exit; 
```

并使用以下命令从远程 shell 执行数据库恢复:

```
# mysql sendy < /srv/db\_bak/backup\_to\_restore.sql 
```

然后在你的浏览器中刷新熊智娟，你应该回到你在数据库损坏之前开始的地方。

### 让我们创建一个水滴的快照

有一个“撤销”点很好。DigitalOcean 使您能够在任何时间点创建虚拟服务器的快照。如果您需要恢复虚拟服务器，这将是创建快照的好时机。如果您这样做了，您只需要将您的数据库重新导入到您的 S3 存储桶中最新保存的版本，您就可以重新开始工作了。

要做到这一点，首先我们需要使用以下命令关闭 shell 中的 droplet:

```
# shutdown 
```

这将为您提供类似于以下内容的输出:

```
Shutdown scheduled for Tue 2019-01-15 04:49:54 UTC, use 'shutdown -c' to cancel. 
```

您的服务器现在正在执行关机脚本，这比硬关机要好。现在导航到[https://cloud.digitalocean.com/droplets](https://cloud.digitalocean.com/droplets)并选择你的熊智娟水滴。您应该会看到它当前处于关闭状态。

[![](img/6708cbb36bdb10a3458c08a9a837a708.png)](https://cdn-images-1.medium.com/max/106/1*yj4ZwLEYrsIGEtfwOj9vvA.png)

[![](img/3e81984bc70a1812050d71dd6d01f4f3.png)](https://cdn-images-1.medium.com/max/116/1*N2H8RMKbQygr8BLb8olgIA.png)

单击“Snapshots”选项卡，为您的快照取一个合理的名称。然后单击拍摄快照，这将需要几分钟时间。

[![](img/4bb3c86c6e79251feabc20a17fffa4d3.png)](https://cdn-images-1.medium.com/max/909/1*kmEnP_uDVq1f5H0eje455g.png)

就本文发表时的报价而言，存储一个快照每月大约需要 0.10 美元。

快照完成后，回到电源菜单，重新打开 droplet。在尝试重新加载之前，给它几分钟时间完全重新启动，否则您可能会看到与连接到服务器或数据库相关的错误。

您可以像这样随时拍摄快照，但是当然要注意当您关闭它时是否有正在运行的作业。

[![](img/58de90e0fe065594206df79ccc566d78.png)](https://cdn-images-1.medium.com/max/605/1*2CUtloFHlWA61O2ngW1m5Q.png)

***恭喜你，就是这样！*T3】**

### 请评论一下进行的如何！我很想听听你的过程如何，以及这个指南是否对你有所帮助。

> 我希望这个指南能为你省去很多我在设置时遇到的麻烦。我很高兴知道它可能对你有所帮助，如果你想说谢谢，请留下评论，[和/或请随意给我买啤酒！](https://paypal.me/alexjacobs)🍻干杯！**