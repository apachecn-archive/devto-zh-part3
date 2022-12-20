# 我如何在大项目中保持高效率

> 原文：<https://dev.to/maciekchmura/how-i-stay-productive-in-a-big-project-388b>

...不要在路上迷路。

## 问题

近一年来，我一直是一个大型成熟的 JavaScript 项目的一部分。没有框架。就 Node，JS，MVC。很多时候，当我修复一个 bug 时，我不得不跳到多个文件和类中进行调查。我的“打开文件”标签很快就满了。我的主要问题是在不同的解决方案之间跳跃。

我想在代码中执行一些更改，测试它，然后留待以后去寻找不同的方法。我重复这些步骤几次。然后，在我看来，当我有了一个最适合的修复方案时，我可以为代码评审做一个 PR 或者和我的团队讨论它。

因此，理想情况下，我希望在可能的修复之间快速切换。对于这一点，我有两种方法。

## 保存一个差异文件

```
git diff > fix1.diff 
```

Git 将生成一个补丁文件，其中包含对存储库所做的所有更改。我可以将这个文件发送给某人，在它自己的窗口中打开它以与当前状态进行比较，等等。
非常容易进行快速验证。

要应用此文件:

```
git apply fix1.diff 
```

这是在提交之间逐步保存工作的最简单的工作流。我有一个包含所有修改的文件。
这很好也很简单，但是还有更好的解决方案。

## Git Stash

储存是把工作留到以后做。关于这个主题有很多很棒的教程和文档。
[大西洋](https://pl.atlassian.com/git/tutorials/saving-changes/git-stash)
T5】git-SCM

我发现这两个命令对我很有帮助:

```
git stash save <message>
git stash apply 
```

将保存更改并清理我的工作目录，所以要继续工作，我必须应用它们。(`git stash pop`也将应用更改，但它们将从 stash 中删除)。

现在，我在工作“时间线”中有了一个保存点，我可以很容易地评估或恢复。这也可以在 VScode 中用 [Gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) 插件完成(Webstorm 也有这个功能)

**更新由突出显示的

[![heberqc image](img/ea1a0e5c89eb68a21482411b7d657128.png)](/heberqc)

## [赫柏·克格亚娜](/heberqc)

[I'm from Perú 🇵🇪, I work as web developer 👨🏽‍💻. I like learning about computing topics.](/heberqc)

[![twitter logo](img/ecef78ee24c258a213354fc0e60fd71a.png)Heber QC](https://twitter.com/heberqc)[![github logo](img/7e90f0f60c25b501324445b96acd3de8.png)Heber QC](https://github.com/heberqc)

`git stash save`已弃用。请使用`git stash push`

## 微提示:个性化评论

我是这样写的:

```
// @mch <what I think is happening here> 
```

*mch >我名字的首字母*

在编辑器中，我设置了一个规则来高亮显示`@mch`字符串。【VScode 有一个不错的插件: [TODO](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)

我用
定制了它

```
"todohighlight.keywords": [
    {
      "text": "TODO",
      "color": "#000000",
      "backgroundColor": "gold",
      "borderRadius": "2px",
    },
    {
      "text": "@mch",
      "color": "#66ffdd",
      "backgroundColor": "#116644",
      "borderRadius": "2px",
    },
  ], 
```

这有助于快速查找所有我能看到的地方。
用`@mch`Ctrl+Shift+F 或者用 TODO 插件查找。

这三个建议对我的日常工作有帮助。你有什么高效工作的诀窍？？

声明:
这篇文章是我的第一篇博客文章，:D
感谢开发团队让我有机会分享:D