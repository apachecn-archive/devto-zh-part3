# 👩‍🎓开源考试模拟器

> 原文：<https://dev.to/benjaminadk/-open-source-exam-simulator--3fd1>

#### [简介](#introduction) ◽ [考试模拟器](#exam-simulator) ◽ [考试制作者](#exam-maker) ◽ [向前进](#moving-forward)

[https://www.youtube.com/embed/KLlTQULbxhU](https://www.youtube.com/embed/KLlTQULbxhU)

### 简介

在拿到大学学位之前，我需要通过的最后一门课是 Oracle 数据库认证。学校提供的学习材料不足以让学生做好准备，一个课程聊天室里满是抱怨失败的人就证明了这一点。我第一次尝试也以 4 分之差失败了🤯。事实证明，学习这些东西的最好方法是找到 *VCE* 文件和运行它们的模拟器。

VCE 的缺点是它通常很昂贵💲💲💲对于文件，模拟器，或某种成员。因为我主修软件开发，所以我决定用 JavaScript 开发自己的开源解决方案。到今天为止，一切都在 alpha 中，我可以使用一些反馈，考试创建者或贡献者来帮助推进项目。

### 考试模拟器

该项目的模拟器部分由*电子*构建，可用于 *Windows* 和 *MacOS* 。我还没有使用/测试过 *MacOS* 版本。模拟器的数据来自存储在用户本地机器上的 JSON 文件。我已经建立了包括考试计时器，4 个问题类型(多选，多答案，填写&拖放列表)，考试复习报告，保存的考试会话，考试验证等功能。

### 考试制造者

尽管可以在任何文本编辑器中创建考试，但我认为让在线考试编辑器创建考试文件并作为所有考试的中央存储库是一个好主意。这个网站是一个 *Next.js* 应用，有一个 *Node.js* 后端和一个 *Prisma* 数据库。它在免费爱好层的 Heroku 上，所以启动起来有点慢。用户创建和分享测试是应用有价值的地方。

### 向前移动

谁知道呢。没人会用这东西🤦‍♂️，但至少工作很有趣。如果您感兴趣，请查看以下资源。不缺少 bug、性能优化和功能创建。

[考试模拟器下载](https://github.com/exam-simulator/simulator/releases)

[考试模拟器文档](https://exam-simulator.gitbook.io/exam-simulator/)

[Heroku 上的考官](https://exam-maker.herokuapp.com/)

## ![GitHub logo](img/292a238c61c5611a7f4d07a21d9e8e0a.png) [考试模拟器](https://github.com/exam-simulator) / [模拟器](https://github.com/exam-simulator/simulator)

### 👩‍🎓基于 JSON 的考试模拟器。

<article class="markdown-body entry-content container-lg" itemprop="text">

# 考试模拟器 JS

基于 JSON 的开源免费考试模拟器。

[![Windows](img/276b0a497d25241b7f33eea9ff5291ec.png) ](https://camo.githubusercontent.com/ab36e474437a428dbcd3fbb69b2751490b143813d63e5c4088ad95640f600411/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d2d77696e646f77732d6c69676874677265792e737667) [ ![AppVeyor](img/2d8f0a4ebedf325c4b8dbd96f394f608.png)](https://camo.githubusercontent.com/1443bd079489883db5fc9352a3c6859909908989751015c29b1f591c023f25e0/68747470733a2f2f696d672e736869656c64732e696f2f6170707665796f722f63692f6578616d2d73696d756c61746f722f73696d756c61746f722e7376673f7374796c653d706f706f7574)

[![Macos](img/12ec81b14b3a4e198ffe5b77da4bb1e2.png) ](https://camo.githubusercontent.com/0d5ea336162d23a66d11978b29ef4c7965a2f501ae9d532c9e246dae2db6c563/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d2d6d61636f732d6c69676874677265792e737667) [ ![Travis (.org)](img/c0154f46f62a09c18e9505ad17dfad9e.png)](https://camo.githubusercontent.com/5350fb828dae698c9da6a3b0eeab89d18ebd09e240487ad97ba90249be7c55e0/68747470733a2f2f696d672e736869656c64732e696f2f7472617669732f6578616d2d73696d756c61746f722f73696d756c61746f722e7376673f7374796c653d706f706f7574)

[![](img/c15d542192484ce535845dc45e2d33bc.png)](https://camo.githubusercontent.com/4516feb8df3c46047a8b5af545bd636d8d7b77881af76c3675630d90422fdb4c/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f646f776e6c6f6164732f6578616d2d73696d756c61746f722f73696d756c61746f722f746f74616c2e7376673f7374796c653d706f706f7574)

### [文档](https://exam-simulator.gitbook.io/exam-simulator)

## 笔记

这个项目是在阿尔法发布。敬请关注大量的改进和更新。

## 捐赠

这个应用程序是免费的，但我不会拒绝捐赠。<g-emoji class="g-emoji" alias="money_mouth_face" fallback-src="https://github.githubassets.cimg/icons/emoji/unicode/1f911.png">🤑</g-emoji>

[![](img/aff386119a93f187976e29437587ba22.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=BKMDFU4LLE6NU&source=url)

</article>

[View on GitHub](https://github.com/exam-simulator/simulator)

## ![GitHub logo](img/292a238c61c5611a7f4d07a21d9e8e0a.png) [考试模拟器](https://github.com/exam-simulator) / [制作者前端](https://github.com/exam-simulator/maker-frontend)

### 为考试模拟器创建、编辑和共享考试。用 Next.js 构建。

<article class="markdown-body entry-content container-lg" itemprop="text">

# 考试制作前端

</article>

[View on GitHub](https://github.com/exam-simulator/maker-frontend)

## ![GitHub logo](img/292a238c61c5611a7f4d07a21d9e8e0a.png) [考试-模拟器](https://github.com/exam-simulator) / [制作者-后端](https://github.com/exam-simulator/maker-backend)

### 阿波罗考试服务器。

<article class="markdown-body entry-content container-lg" itemprop="text">

# 考试生成器后端

[![Travis (.org)](img/045d93a3d850a815b2bfd8c8df59ebaa.png)](https://camo.githubusercontent.com/dff34ef3805fb07fa8d4fcf4dadf2586a9f8142831655ec3a527b3137ab8f79c/68747470733a2f2f696d672e736869656c64732e696f2f7472617669732f6578616d2d73696d756c61746f722f6d616b65722d6261636b656e642e7376673f7374796c653d666c61742d737175617265)

</article>

[View on GitHub](https://github.com/exam-simulator/maker-backend)