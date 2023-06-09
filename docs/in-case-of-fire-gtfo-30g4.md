# 🔥万一发生火灾:GTFO

> 原文：<https://dev.to/chiangs/in-case-of-fire-gtfo-30g4>

## [T1】简介](#intro)

*希望这永远不会发生在你身上，但即使不是火灾或地震，紧急情况也可能出现，你被迫离开办公室/大楼，你还有未提交的代码。*

 **do❓需要什么*

**[都市词典释义](https://www.urbandictionary.com/define.php?term=GTFO)**

 *通常类似于:

```
> git checkout -b emergency-exit
> git add -A
> git commit -m "Emergency exit"
> git push origin head -u 
```

Enter fullscreen mode Exit fullscreen mode

这需要编写大量的命令，很可能会出现拼写错误，并且需要重新输入。我写这个是为了好玩，想分享一下。我有两种方法:git 别名，在 git 别名之上的 Powershell 别名。

### 别名

所以我首先通过编辑`.gitconfig`来创建 git 别名。我想创建一个名为`tfo`的别名，并创建一个连续调用我上面列出的所有 git 命令的函数。

下面是它的样子(通常是一行，但是为了格式化而换行):

```
[alias]
    tfo = "!f() { git checkout -b emergency-exit && git add -A && git commit -m
       'Emergency exit'  && git push origin head -u; }; f" 
```

Enter fullscreen mode Exit fullscreen mode

要在终端命令行中调用它，我只需键入:

```
> git tfo 
```

Enter fullscreen mode Exit fullscreen mode

✨就是这样！我的回购现在有了一个名为`emergency-exit`的分支，我可以在安全的时候合并或者做任何我需要做的事情。

如果我不想写`git` : ***这一步是为 Powershell*** 写的

编辑`.profile`，将`g`别名为`git`。

```
> New-Item alias:g -value git

// or directly in the profile.ps1 file

New-Alias g git 
```

Enter fullscreen mode Exit fullscreen mode

为了调用整个东西，我会输入:

```
> g tfo 
```

Enter fullscreen mode Exit fullscreen mode

💥现在，我可以写一个名为`gtfo`的全局函数来做所有的事情，甚至是一个脚本，但是这又快又简单，而且是一个很好的 10 分钟休息。

** *重要提示* * 】:如果您不幸不得不多次这样做，或者您这样做是为了测试它，请确保在您完成后删除新的分支，否则您会得到一个错误，因为该分支已经存在。您也许可以让它生成带有递增数字或当前日期的分支，以便进行区分。

如果你必须经常这样做...离我远点...女高中生...但是说真的...不要靠近我。

***更新***
如果分支已经在本地存在，确保不会遇到问题的另一种方法是使用`-B`而不是`-b`。如果不存在，这将创建分支，如果存在，则用新的更改重置它。

如果你觉得这很有价值，请留下评论并关注我的[dev . to @ Chiang](https://dev.to/chiangs)和 [Twitter @chiangse](https://twitter.com/chiangse) ，🍻干杯！**