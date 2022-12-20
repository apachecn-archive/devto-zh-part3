# 性能测试弹性研究

> 原文：<https://dev.to/molly/performance-testing-elasticsearch-2f7>

当您有一个大型的 Elasticsearch 集群，并且希望对索引进行更改时，无论是对映射还是设置，您都希望确保这种更改会提高性能。这种改变也可能很难实现，所以您想知道性能的提高是否值得您花费时间来更新代码。

## 我们就做现场！

[![](img/e4e9674a904b35093c6361162082b63a.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--AAdZ-ZRK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/dq2l5qphws8dc2szxqoy.gif)

我不骗你，在早期的肯纳，当我们很小的时候，我们会在我们的生产集群上进行测试。你认为新的碎片设置会更好吗？当然，让我们重新配置一切，看看会发生什么。由于优化出错，我们不止一次地以深夜战斗和救火告终。这是一个陡峭的学习曲线，但最终，我们活了下来！

现在我们更大了，也更聪明了😉，我们已经选择更负责任地进行这些改变，首先对它们进行全面测试！🎉

## 我们如何测试弹性搜索的变化

### TL；速度三角形定位法(dead reckoning)

我们现在使用我们编写的 Ruby [SearchTesting 类](https://gist.github.com/mstruve/ae0d317b6dfd2b08c801822c5df38144)来测试 Elasticsearch 的变化。这个类允许我们在应用程序中同时向**两个不同的索引发出请求。**这两个请求都封装在监控代码中，允许我们在监控服务中跟踪和查看它们。通过跟踪，我们可以比较诸如请求时间之类的东西，看看哪个索引性能更好。下面是我们使用这个类的一个例子。

### 优化:将 id 存储为关键字

在 Elasticsearch 培训中，我们一遍又一遍地被告知，将 id 存储为关键字可以提高搜索性能。原因是因为 Elasticsearch 中的整数数据类型针对范围查询进行了优化。关键字针对术语查询进行了优化。一个术语查询看起来像这样:

```
{  "terms":  {  "id":  [1,  5,  9]  }  } 
```

Enter fullscreen mode Exit fullscreen mode

如果您只使用您的 id 执行术语查询，那么您应该将它们存储为关键字。这听起来像是一个可靠的建议，但更新我们的映射和重新索引我们所有的 30 亿个文档需要一些工作，所以我们想确保在我们做出任何改变之前，我们真的会看到一些好处。

## 测试设置

在我们开始测试之前，我们首先必须为我们的生产索引创建一个副本索引。除此之外，这个新的测试索引将所有的 id 存储为关键字。我们将生产索引中的所有数据重新索引(复制)到新的测试索引中，因此它们都包含完全相同的数据。

接下来，我们必须在 Redis 中放入一个 hash 来通知我们的应用程序我们将测试这个索引。当我们的搜索测试类发出一个 Elasticsearch 请求时，它首先检查 Redis，看看我们是否正在对所请求的索引进行测试。如果是，它将不仅向原始索引发送请求，还向测试索引发送请求。

我们使用下面的方法直接用 SearchTesting 类创建了这个散列。

```
def self.set_test_index(original_index_name, test_index_name)
  index_hashes = cache_index_hashes
  index_hashes[original_index_name] = {
    :index_name => test_index_name,
    :test_searching => true,
    :test_indexing => true
  }
  update_index_cache(index_hashes)
end 
```

Enter fullscreen mode Exit fullscreen mode

在 Redis:
中，产生的散列如下所示

```
{
  "test_indexes" => {
    "prod_index" => {
      "index_name" => "test_index",
      "test_searching" => true,
      "test_indexing" => true
    }
  }
} 
```

Enter fullscreen mode Exit fullscreen mode

一旦散列到位，测试就开始了！我们想用 SearchTesting 类测试两件事，搜索速度和索引速度。

## 测试搜索速度

当我们引入这个新的搜索测试类时，我们以这样一种方式实现它，即我们向 Elasticsearch 发出的每个请求都要经过 it .

```
@connection.send(m, *args, &block)
# was replaced with
SearchTesting.new(@connection).send(m, *args, &block) 
```

Enter fullscreen mode Exit fullscreen mode

这里有一个来自我们的 [elasticsearch-ruby](https://github.com/elastic/elasticsearch-ruby) gem 的连接对象，它允许我们与 elasticsearch 对话。我们将它传递给我们的 SearchTesting 类，然后它定义我们想要测试的方法。任何未定义的方法都可以不受干扰地直接进入 Elasticsearch。

```
def method_missing(m, *args, &block)
  connection.send(m, *args, &block)
end 
```

Enter fullscreen mode Exit fullscreen mode

因为我们想测试搜索方法，所以我们这样定义它:

```
def search(*args)
  self.index_name = args&.first&.dig(:index)
  test_hash = cache_index_hashes.dig(index_name)

  test_search_thread = (Thread.new { test_search(*args.deep_dup, test_hash) } if test_searching_enabled?(test_hash))

  connection.search(*args)
end 
```

Enter fullscreen mode Exit fullscreen mode

这里发生的一些事情我想放大一下。首先，我们得到我们请求的索引的名称。然后，我们检查该索引是否被测试。

```
self.index_name = args&.first&.dig(:index)
test_hash = cache_index_hashes.dig(index_name) 
```

Enter fullscreen mode Exit fullscreen mode

如果 Redis 中存在一个测试散列，并且为该测试散列启用了搜索，那么我们将创建一个新线程来运行测试请求。

```
test_search_thread = (Thread.new { test_search(*args.deep_dup, test_hash) } if test_searching_enabled?(test_hash)) 
```

Enter fullscreen mode Exit fullscreen mode

那个方法非常简单。它所做的只是用测试索引名替换原始索引名，然后对测试索引执行请求。

```
def test_search(*args, test_hash)
  test_index_name = test_hash[:index_name] # fetch test index name
  args.first[:index] = test_index_name # replace original index name with test
  connection.search(*args) # execute request against the test index
end 
```

Enter fullscreen mode Exit fullscreen mode

由于`test_search`方法运行在不同的线程中，我们不必担心等待它完成。一旦我们启动了 test_index 请求，我们就向原始索引发出请求并返回这些结果！

现在，搜索只是画面的一半。我们还想确保索引仍然有效。

## 测试分度速度

为了测试索引速度，我们必须使用批量方法进行测试，因为我们所有的索引都是批量完成的。我们通过创建一个单独的线程来运行测试索引，类似于搜索。

```
def bulk(*args)
  test_bulk_thread = Thread.new { test_bulk(*args.deep_dup) }
  connection.bulk(*args)
end 
```

Enter fullscreen mode Exit fullscreen mode

这里最难的部分是，有时不同索引的散列被组合在一起。因此，我们必须单独检查每个索引散列。如果有任何哈希指向正在测试的生产索引，那么我们也会将这些哈希索引到测试索引。

```
def test_bulk(*args)
  new_bulk_hashes = test_bulk_hashes(*args) # select hashes to go to the test index
  return unless new_bulk_hashes.any? # if there are none return 
  connection.bulk({ :body => new_bulk_hashes }) # index hashes to the test index
end

def test_bulk_hashes(*args)
  @test_bulk_hashes ||= args.first[:body].map do |index_hash|
    index_name = test_index_name(index_hash.values.first[:_index])
    next unless index_name # if there is an index name, a test is running
    index_hash.values.first[:_index] = index_name # replace original index with test index
    index_hash # return new index hash to the map
  end.compact
end 
```

Enter fullscreen mode Exit fullscreen mode

与搜索一样，因为索引是在一个单独的线程中进行的，一旦我们开始它，我们就可以继续将文档索引到原始索引并返回结果。

## 不要着急

**当你在 Elasticsearch 中测试性能指标时，关键之一是给测试运行时间。** Elasticsearch 会在你搜索时建立缓存。这意味着您的原始索引(已经建立了缓存)可能会比您的新索引更快。您希望让测试运行足够长的时间来构建相同的缓存，因此该测试是真正的一对一测试。我们通常让我们的测试运行至少几天。这不仅允许索引有时间建立缓存，还允许进行各种各样的搜索和索引请求。

到目前为止，我们已经使用这个搜索类来测试将 id 更改为关键字和碎片计数的变化。将 id 改为关键字使我们的搜索速度提高了 30%,而索引速度没有变化。减少碎片数量会全面损害我们的性能，所以我们没有在代码库中进行这种改变。

我想分享这段代码，希望其他人可以使用这个类在 Ruby 中设置他们自己的 Elasticsearch 测试！如果你有任何问题或者我没有解释清楚，请不要犹豫地问！