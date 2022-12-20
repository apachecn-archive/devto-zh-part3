# noobs 的时间复杂度

> 原文：<https://dev.to/mercier_remi/time-complexity-for-noobs-48db>

这是一个我从未想过要写的主题。但是生活充满了惊喜。

通过编码训练营学习 web 开发的人(通常)不熟悉时间复杂性。他们可能已经读了这些话，但仅此而已。我知道我试图查阅维基百科页面，结果在第一段结束时睡着了。当我终于醒来时，我想“没关系，反正我永远也不会处理它”。

天哪，我错了。在一次编码面试中，我很快被要求写一个配对算法并计算它的时间复杂度🤔。它让我意识到时间复杂性不仅仅是一个工程师抛出的让你出汗的花哨词汇。了解它的要点在编码面试世界之外也是有用的。

所以，对于我所有的新手伙伴们，这里是时间复杂性的基本原则——这样你就能在下次鸡尾酒会上像钻石一样闪耀。

## noobs 的时间复杂度

> 时间复杂度是根据输入的大小来衡量代码执行时间的方法。

就是这样。你现在可以停止阅读，回到你的生活中去。

还和我在一起吗？好吧，想要 CS 的书呆子，这里有个例子:

*   如果您的代码在相同的时间内处理一个 10 整数的数组和一个 10，000 整数的数组，做得好，您的代码的时间复杂度是👌。
*   如果你的代码处理一个 10 个整数的数组和一个 10，000 个整数的数组要多花 1，000 倍的时间，对不起，你的代码的时间复杂度是💩 <sup id="fnref1">[1](#fn1)</sup> 。

## 一个急需的例子

让我们来看看我必须解决的经典编码面试:

假设你得到一个由 10，000 个随机整数组成的数组，正的和负的。您希望得到所有元素对(没有重复)的总和为给定的数字。

这里有第一个解决方案:

```
 def find_pairs(array, sum)
    array.repeated_combination(2).find_all { |a, b| a + b == sum }.uniq
  end 
```

让我们来分解一下:

*   `repeated_combination(2)`生成一个枚举数，包含 10，000 数组中所有重复的整数对(大约 50，000，000 个元素😱).
*   `find_all`返回一个数组，其中包含所有检查该块的事件。
*   `uniq`删除重复的对

但更重要的是:根据输入的大小，这个方法运行需要多少时间？

我们将用 [benchmark-ips](https://github.com/evanphx/benchmark-ips) 对我们的代码进行基准测试。这个 gem 提供了你的代码每秒运行的次数。

要添加`benchmark-ips`，首先运行:

```
 gem install benchmark-ips 
```

然后将基准添加到您的代码中。这里，我将使用四个不同的输入来运行我的`find_pairs(array, sum)`:

*   由 10 个整数组成的数组
*   一个 100 整数的数组
*   一个 1000 整数的数组
*   一个 10，000 整数的数组

```
 require 'benchmark/ips'

  def find_pairs(array, sum)
    array.repeated_combination(2).find_all { |a, b| a + b == sum }.uniq
  end

  ########################
  # COMPLEXITY BENCHMARK #
  ########################

  # Set up

  FIRST_ARRAY = Array.new(10) { rand(-10...20) }
  SECOND_ARRAY = Array.new(100) { rand(-100...200) }
  THIRD_ARRAY = Array.new(1000) { rand(-1000...2000) }
  FOURTH_ARRAY = Array.new(10_000) { rand(-10_000...20_000) }

  # Time complexity benchmark

  Benchmark.ips do |x|
    x.config(:time => 5, :warmup => 2)

    # Typical mode, runs the block as many times as it can
    x.report("find pairs in 10-integer array") {
      find_pairs(FIRST_ARRAY, 8)
    }

    x.report("find pairs in 100-integer array") {
      find_pairs(SECOND_ARRAY, 87)
    }

    x.report("find pairs in 1000-integer array") {
      find_pairs(THIRD_ARRAY, 876)
    }

    x.report("find pairs in 10_000-integer array") {
      find_pairs(FOURTH_ARRAY, 8765)
    }

    x.compare!
  end 
```

下面是输出:

```
 Warming up --------------------------------------
  find pairs in 10-integer array
                          10.685k i/100ms
  find pairs in 100-integer array
                         137.000  i/100ms
  find pairs in 1000-integer array
                           1.000  i/100ms
  find pairs in 10_000-integer array
                           1.000  i/100ms
  Calculating -------------------------------------
  find pairs in 10-integer array
                          112.738k (± 1.5%) i/s -    566.305k in 5.024381s
  find pairs in 100-integer array
                            1.388k (± 2.2%) i/s -      6.987k in 5.036686s
  find pairs in 1000-integer array
                           14.208  (± 0.0%) i/s -     72.000  in 5.069451s
  find pairs in 10_000-integer array
                            0.138  (± 0.0%) i/s -      1.000  in 7.241235s

  Comparison:
  find pairs in 10-integer array:   112737.7 i/s
  find pairs in 100-integer array:     1388.0 i/s - 81.22x  slower
  find pairs in 1000-integer array:       14.2 i/s - 7934.72x  slower
  find pairs in 10_000-integer array:        0.1 i/s - 816360.51x  slower 
```

😓如您所见，我的代码在 10-整数数组和 100-整数数组之间运行要多花 80 倍的时间。但是，100 个整数的数组和 1000 个整数的数组之间的时间要长 100 倍。1000 整数数组和 10000 整数数组之间的长度是 100 倍。

可以肯定地说，它的时间复杂度是💩。

您已经可以想象当您的代码运行时，用户看着一个空白屏幕 10 秒钟。朋友们，这一点都不好。

## 从“动👌和💩“记数法到大 0 记数法

时间复杂度有自己的标度:大 0 符号 <sup id="fnref2">[2](#fn2)</sup> 。

从最好到最差:

*   输入的大小不会影响代码的运行时间
*   `0(log n)`:10 倍的输入，你的代码需要 2 倍的时间来运行
*   `0(n)`:10 倍的输入，你的代码需要 10 倍的时间来运行
*   `0(n log n)`:10 倍的输入，你的代码需要 50 倍的时间来运行
*   `0(n^2)`:10 倍的输入，你的代码需要 100 倍的时间来运行
*   `0(2n)` : 10x 输入，你的用户已经睡着了，而你的代码还在运行

真的没有必要背下来或者能够在你的头顶上计算出正确的大 0(第一个原因是: [CS 工程师和数学书呆子甚至在微积分](https://www.reddit.com/r/programming/comments/1dotwe/bigo_cheat_sheet/c9si8pj/)上意见不一致)。

最好了解一些经验法则:

*   `n`是指**的输入大小**
*   如果在一个数组上迭代一次，运行时间与数组中元素的数量成线性比例(因此为`0(n)`)
*   具有 2 级嵌套循环的代码通常符合`0(n^2)`(具有 3 级嵌套循环的代码将符合`0(n^3)`等等)
*   时间复杂度表示基于输入大小的可能的性能问题

如果你想得到更深入的解释，去看看 Honeybadger 的《Rubyist 的 Big-O 符号指南》。很整洁👌。

## 回到我们现实生活中的例子

记得吗？

```
 def find_pairs(array, sum)
    array.repeated_combination(2).find_all { |a, b| a + b == sum }.uniq
  end 
```

计算一段代码的时间复杂度，你只需要定义每个元素的时间复杂度，最差的那位胜出。所以让我们来分解一下`find_pairs()`看看引擎盖下到底发生了什么。

#### ☝️首先，让我们创建一个 4-整数数组

```
 array = Array.new(4) { rand(-10...20) } # => [-6, 4, 14, 3] 
```

#### ✌️然后，让我们定义一个方法`find_pairs()`并在数组上调用`repeated_combination(2)`来创建对

```
 array = Array.new(4) { rand(-10...20) } # => [-6, 4, 14, 3]

  def find_pairs(array, sum)
    # Calling repeated_combination without a block returns an enumerator
    array.repeatead_combination(2)
  end

  # I add a to_a to transform my enumerator into an array for readability reasons
  find_pairs(array, 8).to_a # => [[-6, -6], [-6, 4], [-6, 14], [-6, 3], [4, 4], [4, 14], [4, 3], [14, 14], [14, 3], [3, 3]] 
```

让我们在那里停一会儿。`repeated_combination(2)`遍历`array`中的每个元素，并将其与`array`中的每个元素配对。`repeated_combination(2)`创建一个嵌套循环。

第一个循环首先在`-6`处停止，然后第二个循环开始并遍历每个元素。一旦第二个循环完成，我们回到第一个循环，第一个循环移动到`4`上，第二个循环再次开始。清洗并重复，直到`repeated_combination(2)`遍历完每个元素并返回所有组合。

循环中的循环？👉该方法的时间复杂度为`0(n^2)`

#### ☝️✌️现在让我们`find_all`对其整数之和为一个给定的数。

```
 array = Array.new(4) { rand(-10...20) } # => [-6, 4, 14, 3]

  def find_pairs(array, sum)
    all_pairs = array.repeatead_combination(2).find_all { |a, b| a + b == sum }
  end

  find_pairs(array, 8) # => [[-6, 14], [4, 4]] 
```

`find_all`遍历由`repeated_combination(2)`返回的枚举器，并检查与块匹配的对。

单循环表示`0(n)`。由于`0(n)`比`repeated_combination(2)`的`O(n^2)`快得多，所以在大的计划中，它将是无关紧要的。

#### ✌️✌️最后，让我们用`uniq`去掉重复的

```
 array = Array.new(4) { rand(-10...20) } # => [-6, 4, 14, 3]

  def find_pairs(array, sum)
    all_pairs = array.repeatead_combination(2).find_all { |a, b| a + b == sum }.uniq
  end

  find_pairs(array, 8) # => [[-6, 14], [4, 4]] 
```

在本例中，我们没有任何副本。但是当你在一个 10，000 整数的数组上运行这个算法时，你会有一些机会。

`uniq`只在数组上迭代一次，所以它的时间复杂度是`0(n)`。还是那句话，不会对整个方法的时间复杂度有太大影响。

#### 🖐捣鼓着数字

现在我们知道了每个元素的时间复杂度，让我们来计算整个方法的复杂度。

`0(n^2)` + `0(n)` + `0(n)` = `0(n^2)`

正如我之前所说的,`0(n)`比`0(n^2)`快得多，不会对最终的时间复杂度产生实质性的影响。

所以现在，我知道我的代码的时间复杂度是`0(n^2)`。你可以坚持我的💩notation but `0(n^2)`会让你看起来稍微聪明一点。

这就是了解时间复杂性的要点派上用场的地方。如果您知道当输入增加时，您的代码会成倍地变慢，那么您就知道当您扩展时会有性能问题。所以你必须让你的代码更好。

## 让算法更好

我将使用 [Set](https://devdocs.io/ruby~2.5/set) ，而不是使用 Ruby 的一些内置方法。Set 创建一个没有重复的无序值集合，具有快速查找功能。

```
 require 'set'

  def find_pairs(array, sum)
    results = Set.new
    input = array.to_set

    input.each do |element|
      other_element = sum - element
      if input.include?(other_element)
        pair = element > other_element ? [other_element, element] : [element, other_element]
        results << pair
      end
    end
    results
  end 
```

让我们来分解一下:

*   我创建了一个空集来存储`results`。
*   我将输入数组转换成一个集合，并存储到`input`中。
*   我迭代了`input`。
*   我取第一个整数(`element`)并从`sum`中减去它。这给了我一个潜在的配对整数(`other_element`)。
*   如果`other_element`包含在`input`中(我的输入数组变成了一个集合)，那么我确保`element`和`other_element`从最小到最大配对，并推入`results`。为什么要费心去整理它们呢？因为如果我有重复的，`results`作为一个集合只会保留第一个出现的👌。

下面是这个新代码的基准:

```
 Warming up --------------------------------------
  find pairs in 10-integer array
                           8.171k i/100ms
  find pairs in 100-integer array
                           1.533k i/100ms
  find pairs in 1000-integer array
                         146.000  i/100ms
  find pairs in 10_000-integer array
                          14.000  i/100ms
  Calculating -------------------------------------
  find pairs in 10-integer array
                           80.106k (± 8.6%) i/s -    400.379k in 5.037247s
  find pairs in 100-integer array
                           14.005k (±12.4%) i/s -     68.985k in 5.026938s
  find pairs in 1000-integer array
                            1.540k (± 3.8%) i/s -      7.738k in 5.033787s
  find pairs in 10_000-integer array
                          157.341  (± 2.5%) i/s -    798.000  in 5.074419s

  Comparison:
  find pairs in 10-integer array:    80106.5 i/s
  find pairs in 100-integer array:    14005.1 i/s - 5.72x  slower
  find pairs in 1000-integer array:     1539.6 i/s - 52.03x  slower
  find pairs in 10_000-integer array:      157.3 i/s - 509.13x  slower 
```

好多了！现在，当输入乘以 10 时，我的代码只需要多运行 10 倍。它的时间复杂度已经变成线性的(`0(n)`)，并且处理 10，000 个整数的输入🥳.需要不到 0，006 秒

好吧，那真是超级极客。我希望它能帮助其他训练营成员理解时间复杂性以及它的用处。

我错过了什么吗？让我在推特或开发✌️.上知道

雷米

* * *

1.  不是真正的开发者符号。 [↩](#fnref1)

2.  一个**Real Developer**符号。 [↩](#fnref2)