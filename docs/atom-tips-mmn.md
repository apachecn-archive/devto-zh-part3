# Atom Tips 🌶

> 原文：<https://dev.to/nervewax/atom-tips-mmn>

*本文最初发布于 2018 年 9 月 16 日[nervewax.com/atom-tips](https://nervewax.com/atom-tips/)T3】*

我几乎每天都在使用 Github 的文本编辑器 [Atom](https://atom.io/) ,所以我想也许我应该为那些刚刚开始使用它的人，甚至是那些考虑转而使用它的人写一些提示。

## 强力键

### 命令调色板

这个可能是最有用的，它打开了 atom 命令面板。Atom 自己在[飞行手册](https://flight-manual.atom.io/getting-started/sections/atom-basics/)中描述得最好:

> 这个搜索驱动的菜单可以完成 Atom 中可能的任何主要任务。您可以按下 Cmd+Shift+P 来搜索命令，而不是单击所有应用程序菜单来查找内容。

### 模糊查找器

当你有一个疯狂的大项目时，这可以帮助你快速找到并打开一个文件。它使用“模糊”搜索，因此结果可以匹配搜索查询的任何部分。它还默认跳过 git 忽略的文件，这很酷。

### 找到&替换

对于这个熟悉的功能，Atom 的快捷键有两种风格。前者只在当前标签页中搜索，但是按住 shift 键，你可以搜索整个打开的项目。

除此之外，您可以以文件模式的形式提供第三个参数，因此输入`*.html`将只搜索 HTML 文件等。还有一个正则表达式开关...但是我们不要去那个兔子洞。

## 套餐

目前有近 8000 个 atom 包可用，所以很难找到真正有用的。我长话短说，以下是我最喜欢的包:

### 埃米特

你可能对这个很熟悉，因为它在其他一些文本编辑器中也有。基本上，它让你像这样改变代码:

```
ul.list>li.list__item*5>a.list__link 
```

成这样:

```
<ul class="list">
  <li class="list__item"><a href="" class="list__link"></a></li>
  <li class="list__item"><a href="" class="list__link"></a></li>
  <li class="list__item"><a href="" class="list__link"></a></li>
  <li class="list__item"><a href="" class="list__link"></a></li>
  <li class="list__item"><a href="" class="list__link"></a></li>
</ul> 
```

非常适合快速准备标记，现在不使用它编码会感觉很慢！🐌

### 树形视图 Git 状态

[tree-view-git-status](https://atom.io/packages/tree-view-git-status) 完全按照 tin 上所说的做，它将当前的 git 分支和状态添加到您的树视图中。尽管 Atom 最近在右下角的状态区加入了这些信息，但我觉得树形视图中提供的颜色和文本仍然很方便。

### 颜料

[颜料](https://atom.io/packages/pigments)在编辑器中应用颜色的颜色值作为该文本的背景。有点像这样: rgb(25，187，134) -所以是的，不用切换到浏览器就可以很方便地看到你在做什么。

### 文件图标

[文件图标](https://atom.io/packages/file-icons)只是把默认的文件图标换成一个代表文件类型的小图标。惊人的有用

### 原子排列

原子排列很简单，但是 T2 很有用。它只是一个键盘快捷键，可以让你快速对齐所有的变量定义。这可以节省你自己调整它们的努力，但是你也可以看到所有的东西都很好的排列起来，从而获得一点满足感。🤓

## 原子外壳命令

这是一个很好的特性，如果您经常在编辑器和终端之间切换，安装 atom shell 命令，您就可以使用`atom .`在 atom 中打开 cwd。执行`atom -h`查看所有其他可用命令👌

Atom package manager 还可以让你用`apm install [package]`安装包，这对于在新机器上快速安装也很方便。

## 圣杯🍹

把最好的留到最后， [Atom v1.18](http://blog.atom.io/2017/06/13/atom-1-18.html) 推出了我最喜欢的新功能，Git/Github 标签。

这个侧面板为您提供了本地 git 分支当前状态的可视化视图。从这里，您可以轻松地导航更改、解决合并冲突、拉/推等等！

我不会说太多，但是这个特性很好地改善了我的工作流程，所以我会推荐 Atom。

[![Atom Wizard](img/c1118d8a0656c2bff31ba308988b4751.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--MtZrIsbG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://nervewax.com/conteimg/2018/02/rWu0kFq1.jpg)

所以现在你是一个真正的原子巫师！🧙 [让我知道](http://twitter.com/nervewax)没有什么*你*不能编码！