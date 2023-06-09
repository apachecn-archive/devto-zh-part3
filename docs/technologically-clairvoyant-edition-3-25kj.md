# 🔮技术透视第 3 版

> 原文：<https://dev.to/brandonskerritt/technologically-clairvoyant-edition-3-25kj>

这是一份每周时事通讯。你可以在这里报名

# **你好👋**

我是布兰登。

🌊欢迎来到第三版🔮技术透视🔮

本周没有公告，所以让我们开始吧🎪

## 不久的将来

🚅[荷兰的铁路系统使用 WiFi 追踪](https://www.ns.nl/en/privacy/in-and-around-the-station.html?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

继上周之后，荷兰公开承认使用 WiFi 追踪。然而，与伦敦交通局不同，他们更详细地说明了为什么和如何。首先，让我们谈谈安全问题。MAC 地址是唯一可识别的地址，可以识别网卡。通常每个设备都有一个网卡，所以它们可以用来识别设备。当他们跟踪你的设备时，MAC 地址被散列，然后被发送到服务器，在那里被存储。盐每天都在变化，并没有储存在电脑里。然后他们切掉一些散列值，这样就没有办法通过散列 MAC 地址追踪到个人。他们使用的哈希算法是 Bcrypt。我不得不承认，在火车站的本地哈希是天才。

通常情况下，客户端哈希很糟糕。当您对客户端进行哈希处理时，哈希后的密码将成为实际的密码。这意味着您将在数据库中存储纯文本(哈希密码就是纯文本)。

但是，MAC 地址是不同的。我们知道它们完全是独一无二的，可以用来识别单个设备。通过在客户端对它们进行散列，散列将成为 MAC 地址的 id。这意味着在服务器上，没有办法获得 MAC 地址。你只能得到散列，它不能给出你拥有什么设备的信息。

完全没有办法识别你的个人身份，同时保留通过电台跟踪个人设备的好处。从这种技术中可以明显看出，他们在保持利益的同时，对个人隐私进行了大量的考虑。

他们还承认使用了蓝牙追踪，这种技术已于去年停止使用。如果你对此感兴趣，请点击链接。他们详细说明了为什么要跟踪人，干什么，数据是如何存储的，存储了多长时间。这是一个典型的 GDPR 设置。如果你要求的话，所有的公司都必须给你这些信息，他们已经在网上公布了。如果我看到 GDPR 询问地铁系统上的 w if I 追踪是为了什么/他们是如何做到的，我会让你知道。

🧚‍♀️ [乳腺癌人工智能胜过传统方法](https://news.mit.edu/2019/using-ai-predict-breast-cancer-and-personalize-care-0507?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

人工智能的准确率为 31%，而传统方法的准确率为 18%。这篇文章的另一个有趣之处是:

> “黑人女性死于乳腺癌的可能性高出 42%”

麻省理工学院的研究人员认为这是由于缺乏医疗保健。他们希望他们的人工智能可以在世界各地实施，为各种女性提供廉价的筛查。

🍼每秒生产一百万个塑料瓶**。只有 9%被回收**

 **塑料瓶需要 400 年才能自然分解。如果我们的塑料瓶创造速度继续下去，每 400 年(当一个瓶子分解时)将产生 126100000000000 个塑料瓶。据估计，到 2050 年，海洋中的塑料将比鱼还多。

☁ [5g 频率设定扰乱天气预报](https://www.nature.com/articles/d41586-019-01305-4?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

这不是 5G 直接导致的。5G 计划使用的频率会干扰卫星检测水汽，干扰部分天气预报(但不是所有天气预报)。联邦通信委员会和美国国家海洋和大气管理局正在合作，以确保公司不使用可能影响天气预报的频率。说到 5G，让我们来解决一些其他的 5G 问题。

**5g 能杀死你吗？**

不，它不会杀了你。它会伤害你吗？非常非常不太可能。我将列出一些人们用来证明 5G 会伤害我们的事情，并客观地看待每一件事。

**参议院 637 号法案**

这是美国的法律。许多新闻文章/阴谋论者声称这是参议员们在努力保护我们免受 5G 的威胁，而这项法律并没有实施。

事实证明，这项法律已经出台。它被批准了。但该法案不是关于手机信号塔的“危险”。在这里，你可以读到:

> 该法案将限制州和地方政府对这些“5G”网络征收的分区、许可和其他费用的金额。

这项法律限制了州政府和地方政府收取的许可费、分区费和其他费用。这与 5G 的“危险”无关。你可以在这里阅读。

**[5g 试验杀死数百只鸟](https://www.snopes.com/fact-check/5g-cellular-test-birds/?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)**

假的。最初的消息来源是艾琳·伊丽莎白。它被发布到她的博客“健康坚果新闻”上。同一篇博客声称疫苗会导致自闭症和其他奇怪的事情。有趣的是，她的网站致力于销售产品，以保护你免受所有这些'有害'的事情。

**5G 能杀死人类吗？**

绝对不行。这太荒谬了。首先，[在欧洲有一个 20 - 30 年的测试，对 30，000 人进行测试，监测手机对人类的危害程度](https://www.ukcosmos.org/?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)。这项研究仍在进行中，但到目前为止他们什么也没发现。其次，像这样的基础设施是经过密集测试的。

4G 是在 90 年代末/21 世纪初宣布的。2002 年正式公布。我个人 4、5 年前就有 4g 了。对于他们来说，这是一个非常长的时间来测试事情，让事情变得正确。20 多年来，许多网络已经被宣布，但仍然没有生效。

即使 5G 是危险的，它也会在你购买第一部支持 5G 的手机之前被修复。现在，我建议你自己去读。不要相信你在网上读到的任何东西，包括这篇时事通讯。找到最初的源头，永远。像我在这里所做的那样，主观地测试源代码。

👩‍🍳 [Deepfake Youtube 频道](https://www.youtube.com/channel/UCKpH0CKltc73e4wh0_pgL3g?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

这个 YouTube 频道致力于创建和分发 Deepfake 视频。虽然没有技术突破，但我不得不承认有些视频很有趣😂

🍆[软件开发商开发面部识别软件来寻找色情人物的社交媒体链接](https://www.vice.com/en_us/article/9kxny7/diy-facial-recognition-for-porn-weibo?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

他们声称目前已经找到了 10 万名色情明星的身份。澄清一下，没有证据证明他做过这件事。然而，这在 Twitter 和其他社交媒体网站上引起了不小的轰动。作为一名计算机科学家，这绝对有可能是真的。正如我们两周前看到的，面部识别正在机场用于护照。使用他们的多个图像(通过视频)很有可能找到某人的社交媒体账户。

这位用户在中国网站微博上发布了这条消息。我很难找到原始来源，因为它没有在英文谷歌上为我索引。不过这里有[原出处的截图](https://twitter.com/yiqinfu/status/1133215940936650754?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)。

🔋[中国政府声称新技术突破推动锂生产价格下降 817%](https://electrek.co/2019/05/15/china-lithium-production-breakthrough/?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

该报告声称，它使提取锂的成本(通常为每吨 2 万英镑)降至 2180 英镑，下降了 817%。这可以降低电动汽车的成本。但是，随着中美之间的贸易战，看起来美国电动汽车制造商将不得不等待生产成本的下降。

🤖🚗[研究显示自动驾驶汽车并不比手动挡汽车更安全](https://www.theinformation.com/articles/studies-dont-support-elon-musks-autopilot-safety-claims?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

这篇文章没有确凿的数字证明自动驾驶汽车不安全，然而，这就是问题所在。特斯拉拒绝公布其事故数据。特斯拉发布的数据显示，自动驾驶汽车正变得越来越安全，但或许目前它们并不像行业希望的那样安全。虽然我支持自动驾驶汽车，但很明显，这些汽车在目前的状态下不适合在大多数地方使用。

# 遥远的未来

💉[自己动手做胰岛素:生物黑客旨在抵消飞涨的价格](https://www.dw.com/en/do-it-yourself-insulin-biohackers-aim-to-counteract-skyrocketing-prices/a-48861257?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

70/80 年代的黑客可以做各种很酷的事情。免费长途电话、免费有线电视等等。我相信我们正处于生物黑客革命的边缘。全球数百万糖尿病患者无法获得胰岛素。如果他们这样做，可能要花钱。生物黑客的目标是创建一个“自己做胰岛素”的协议。

🗣 [自然语言处理治愈癌症](https://people.csail.mit.edu/regina/talks/CNLP.pdf?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

这是一场非常精彩的演讲。这个小组的短期临床目标是减少不必要的手术数量。用他们的话说:

> “80%的高风险活检是良性的”

我们知道 AI 可以比人类更好地预测癌症，但它只是没有数据集。这个团队正在使用 NLP 处理文本文档、机器(如心率监测器)生成的报告以及每一份病历。利用这个庞大的数据集，他们将训练一个神经网络来预测癌症。

看着我的水晶球🔮我可以想象这个有更多的用途。想象一下，你戴着一个健身追踪设备，可以追踪你的每一件事。你也可以追踪食物和水的摄入量。所有这些都将结合医生的报告和以往的医疗数据。在后台，机器将使用 NLP 来处理所有这些文档。

使用神经网络，机器将对世界上的每一种疾病进行测试。有些疾病在第一次症状出现之前已经存在多年了。想象一下，如果这个统一的机器在后台 24/7 全天候检查你，将会拯救多少生命。

让我们更进一步。我不想让某个机器知道我所有的私人健康信息？我希望所有的都被加密。我想要它，所以如果他们被黑了，没人能读它。你如何在加密的东西上训练一个神经网络？

你可以使用[全人类加密](https://en.wikipedia.org/wiki/Homomorphic_encryption?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)。您可以在不知道数据是什么的情况下对数据执行计算。E(a) * E(b) = E(a * b) -其中 E(x)是应用于 x 的加密函数。想象一下，未来能够在不知道任何个人数据的情况下训练神经网络。当然，这是在看我现在的水晶球。据我所知，没有完全同态的加密方案。还没有人发表过这样使用它的文章。但 100 年后会很酷。

👻[微型 3d 打印机打印耿鬼](https://www.youtube.com/watch?v=Ohm6S3xFvno?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

Andrew Birch(单人黑客秀)创造了一台非常小的 3D 打印机，并通过打印耿鬼进行了演示。3D 打印机看起来大约有一英尺大小。我可以想象有一天，每个人都带着 3D 打印机到处走，打印他们需要的东西。需要螺丝刀吗？别担心，我现在就打印一份。

🐌Crispr 被用来创造世界上第一只左蜗牛

大多数蜗牛天生是对的。也就是说，它们的壳向右卷曲。事实上，几乎所有的蜗牛都是。[左蜗牛往往是孤独的](https://www.nytimes.com/2017/10/12/science/jeremy-lefty-snail.html?module=inline?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)。

这项技术对于理解为什么 1 万人中有 1 人生来就患有先天性内翻畸形(T1)很重要，这种情况下，他们的内部器官像左撇子的蜗牛壳一样翻转。

🚀[美国宇航局与维珍公司合作，为火箭制造 3d 打印燃烧室](https://www.nasa.gov/centers/marshall/news/news/releases/2019/nasa-and-virgin-orbit-3d-print-test-rocket-combustion-chamber.html?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

传统上，制造、测试和交付传统的燃烧嵌合体需要几个月的时间。这项新技术有望大大缩短这一时间。

🖨 [新型低成本生物打印机](https://www.instructables.com/id/Low-Cost-Bioprinter/?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

生物打印是一种附加程序，其中将细胞和生长因子等生物材料结合起来，以创建模仿自然组织的组织样结构。通常低端生物打印机的价格为 1 万英镑。而高端的要 17 万。这台打印机可以用 375 英寸制造。

🚀[喷气背包是真的](https://www.youtube.com/watch?v=Rq9hCJwJ7MQ?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

喷气背包是真的。就像电影里一样。但是，问题太多，利大于弊。

首先，人类不应该飞。我们完全不符合空气动力学，需要相对较大的力才能升到空中。第二，喷气背包使用的燃料和火箭使用的燃料是一样的。将氢和氧化剂结合。正如我们可以想象的那样，火箭燃料对环境不是很好。

第三，它们飞的时间不长。上限似乎是 90 秒左右。喷气背包的最快速度是每小时 32.02 英里。

🐝机器人蜜蜂可以给花朵授粉，在水下游泳，还可以粘在表面上

机器蜜蜂存在。它们可以给花授粉，在水下游泳(出于某种原因),像普通蜜蜂一样粘在表面。科学家们已经看到了蜜蜂的大规模灭绝，并决定只制造机器蜜蜂。

一方面，机器蜜蜂很酷。它们可以全天候授粉。另一方面…我不明白为什么科学家们决定直接取代蜜蜂，而不是试图拯救它们。在遥远的未来，认为我们地球上所有的动物都被机器人取代会很可笑吗？

我们可以代替狗、蜜蜂、鱼、猫和各种动物。在遥远的未来，我可以想象取代动物的呼声。“蜜蜂会睡觉，我们的机器人不会。它们比蜜蜂做得更多”。

> “狮子在生命周期中至关重要，但它们杀死了如此多的人类。想想我们的机器狮子，它们学会了不伤害人类”

👓 [Varjo 公布 Xr-1 耳机](https://vrscout.com/news/varjo-xr-1-human-eye-resolution/?utm_source=technologicallyclairvoyant.com&utm_medium=email&utm_campaign=Technologically_Clairvoyant_newsletter)

据报道，沃尔沃使用这种耳机来测试/帮助制造汽车。让我印象深刻的是在汽车内使用 AR 的想法。人工智能可以比人类开得更好，但有时人类比人工智能看得更清楚。使用 AR，我可以想象汽车的挡风玻璃被增强，以提供关于环境的细节，使司机能够用人工智能驾驶*-而不是人工智能或人类驾驶。*

🛴加州大学伯克利分校推出跳跃机器人萨尔托

这种微型机器人跳来跳去，而不是走路或开车。令我惊讶的是，在我见过的所有机器人中，这个机器人表现出了最多的情感——尽管只会跳。

直到下一次，

*   布兰登🐝**