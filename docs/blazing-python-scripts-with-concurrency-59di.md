# ⚡ ️Blazing 蟒蛇🐍具有并发⚡️️的脚本

> 原文：<https://dev.to/theghostyced/blazing-python-scripts-with-concurrency-59di>

为了确保应用程序的高性能，程序员需要编写高效的代码。代码效率与软件的算法效率和运行时执行速度直接相关。

这🐍 [Python 语言是一种🐌缓慢的语言——与 C 或 FORTRAN](https://www.huffpost.com/entry/computer-programming-languages-why-c-runs-so-much_b_59af8178e4b0c50640cd632e?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAIPUUFGtMUrtIlSm2M6P6eg2CjZDE-OL4Gpn1rQXa6X6jwj7N9U5bY0uLxMoOKMlddh_fZUiLy1o6T2VnD0luBVHhyDFf8FBX2axU1a7C-IXGYRGjX3CC4U1VThstAf-ztgbBajuWIBuS18MducDdiTdkjwYrChPyIoETRRxUec6) 相比，为了解决这个问题，人们开发了各种方法来帮助提高编程语言的速度。

一种方法是`CONCURRENCY`。

**并发**🤷‍♂️

并发是指两个任务可以在重叠的时间段内开始、运行和完成。这并不意味着它们会同时运行(*变成 [`Parallelism`](https://wiki.haskell.org/Parallelism_vs._Concurrency)* )。还是很困惑😕？让我们描绘一个场景:

我们正在为一对特殊的夫妇策划一场梦幻婚礼。由我们支配的有玛丽、苏珊、马克斯和西蒙。梦想中的婚礼需要蛋糕、乐队、装饰品和请柬。我们把烘烤蛋糕的任务分配给苏珊，把雇佣乐队的任务分配给西蒙，把布置装饰品的任务分配给玛丽，把请帖发给马克斯。

这四个朋友(*或处理器*)都同时执行他们的任务(*或进程*)，没有任务切换或中断，直到任务完成。这就是——*在外行人看来*的术语简称**并发**。

**PYTHON 中的并发类型**🐍

Python 中的基本并发类型包括:-

*   多线程🧵
*   多重处理🧩( *是的，我知道那是一个拼图块*😏)
*   阿辛西奥⏰(*在另一个教程中有更多关于这个的内容*)

**多线程** 🧵

一个`Thread`是一个操作系统中最小的*执行单元。线程是程序将自己分成两个或多个同时运行的任务的一种方式。线程本身不是程序，它只在一个特定的程序或`process`中运行。*

[![multithreading example](img/643be0f25b0577b5ec04b4bb21793686.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--GUD0sp9M--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/1600/1%2AvZ_RTovr8GW1y7bHBiRQ_A.png)

多线程改变了游戏规则，主要用于 [I/O 绑定操作](https://stackoverflow.com/questions/868568/what-do-the-terms-cpu-bound-and-i-o-bound-mean)。它是一个[中央处理器(CPU)](https://en.wikipedia.org/wiki/Central_processing_unit) (或多核处理器中的一个内核)在操作系统的支持下，同时提供多个执行线程的能力。每个线程共享由它所在的进程提供的相同资源。

让我们举例说明一个多线程操作的例子。首先，我们看一下同步过程。

**同步进程**

```
# A simple python script that gets query a list of site import requests
import time

def get_single_site(url):
    with requests.get(url) as response:
        print(f"Read {len(response.content)} from {url}")

def get_all_sites(sites):
    for url in sites:
        get_single_site(url, session)

if __name__ == "__main__":
    start_time = time.time()
    urls = [
        "https://www.google.com",
        "https://www.facebook.com",
        "https://www.twitter.com/theghostyced"
    ] * 30

    get_all_sites(urls)
    end_time = time.time() - start_time

    print(f"Downloaded {len(sites)} in {end_time} seconds") 
```

这是一个简单的 python 程序，可以下载所提供网站的内容。下载完网站内容后，它会打印出访问过的网站数量和所用的时间。
该脚本利用了 [`requests`](https://2.python-requests.org//en/master/) 库和内置的 python 标准时间库。

运行代码的输出是:

```
...
Read 107786 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107786 from https://www.facebook.com
Read 608077 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107787 from https://www.facebook.com
Read 608077 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107351 from https://www.facebook.com
Read 608311 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107507 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107918 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107149 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11365 from https://www.google.com
Read 107445 from https://www.facebook.com
Read 608077 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107351 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11369 from https://www.google.com
Read 107482 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Downloaded 90 in 17.5553081035614 seconds 
```

在这里，脚本需要 17.5 秒来完成任务。现在让我们再试一次，看看我们是否可以使用多线程的方法来加速它。

**多线程进程**

```
# A simple python script that gets query a list of site import requests
import time
import concurrent.futures

def get_single_site(url):
    with requests.get(url) as response:
        print(f"Read {len(response.content)} from {url}")

def get_all_sites(sites):
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        executor.map(get_single_site, sites)

if __name__ == "__main__":
    start_time = time.time() # our scripts start time
    sites = [
        "https://www.google.com",
        "https://www.facebook.com",
        "https://www.twitter.com/theghostyced"
    ] * 30

    get_all_sites(sites)
    end_time = time.time() - start_time

    print(f"Downloaded {len(sites)} in {end_time} seconds") 
```

在上面的代码中，我们从 Python 标准库中导入了 [`concurrent.futures`模块](https://docs.python.org/3.4/library/concurrent.futures.html?highlight=concurrent.futures#module-concurrent.futures)。该模块有一个 Executor 类，这是`ThreadPoolExecutor`子类的来源。让我们来分解一下`ThreadPoolExecutor`。

所有的`ThreadPoolExecutor`子类所做的只是创建一个线程的 [`Pool`](https://docs.python.org/3.4/library/multiprocessing.html?highlight=pool#module-multiprocessing.pool) 。执行器部分控制线程池中每个线程的运行方式和时间。

上述脚本的输出如下所示:-

```
...
Read 608312 from https://www.twitter.com/theghostyced
Read 11354 from https://www.google.com
Read 107810 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11343 from https://www.google.com
Read 107823 from https://www.facebook.com
Read 608312 from https://www.twitter.com/theghostyced
Read 11326 from https://www.google.com
Read 107388 from https://www.facebook.com
Read 11350 from https://www.google.com
Read 608312 from https://www.twitter.com/theghostyced
Read 107787 from https://www.facebook.com
Read 608311 from https://www.twitter.com/theghostyced
Read 608077 from https://www.twitter.com/theghostyced
Read 11299 from https://www.google.com
Read 11367 from https://www.google.com
Read 608312 from https://www.twitter.com/theghostyced
Read 107785 from https://www.facebook.com
Read 11321 from https://www.google.com
Read 107800 from https://www.facebook.com
Read 107350 from https://www.facebook.com
Read 608076 from https://www.twitter.com/theghostyced
Read 608312 from https://www.twitter.com/theghostyced
Read 608312 from https://www.twitter.com/theghostyced
Read 608311 from https://www.twitter.com/theghostyced
Downloaded 90 in 6.443061351776123 seconds 
```

这里，脚本完成任务需要 6.4 秒。相比之下，同步运行代码需要 17.5 秒。你可能会对自己说——只差 12 秒，我可以接受。假设我们有大量的数据，比如说 1000 个，那么你会明显看到两种方法的不同。

* * *

**多重处理** 🧩

一个**进程**仅仅是一个正在运行的程序的实例。通俗地说，当我们把我们的计算机程序/脚本写在一个文本文件中，执行这个程序，就变成了一个执行程序/脚本中提到的所有任务的进程。A `Process`不与其他进程如`Threads`共享任何内存空间。

多重处理包括在一个计算机系统中使用两个或多个内核。默认情况下，Python 不支持多线程🐍编程语言源于 [`GIL or Global Interpreter Lock hindrance`](https://dev.to/docoprusta/explain-python-global-interpreter-lock-gil-like-im-five-25k8) 。

## GIL 兵强马壮

Python 是由吉多·范·罗苏姆在 20 世纪 80 年代开发的，当时，计算机只使用单个 CPU，为了增加内存管理，Guido 实现了 GIL，只允许一个线程控制 Python 解释器。这意味着，利用一个以上的 CPU 内核或独立的 CPU 来并行运行线程是不可能的。

[`multiprocessing module`](https://docs.python.org/3.4/library/multiprocessing.html) 的引入就是为了绕过这一点。

> **NB**-*GIL 不会阻止多线程的创建。GIL 所做的就是确保一次只有一个线程在执行 Python 代码；控制仍然在线程之间切换。如果你还有困惑， [`this article will definitely help you out`](https://dev.to/docoprusta/explain-python-global-interpreter-lock-gil-like-im-five-25k8)* 。

让我们来说明如何使用上面的同步代码编写多处理操作。

**同步进程**

```
# A simple python script that gets query a list of site import requests
import time

def get_single_site(url):
    with requests.get(url) as response:
        print(f"Read {len(response.content)} from {url}")

def get_all_sites(sites):
    for url in sites:
        get_single_site(url, session)

if __name__ == "__main__":
    start_time = time.time()
    urls = [
        "https://www.google.com",
        "https://www.facebook.com",
        "https://www.twitter.com/theghostyced"
    ] * 30

    get_all_sites(urls)
    end_time = time.time() - start_time

    print(f"Downloaded {len(sites)} in {end_time} seconds") 
```

**多重处理方法**

```
# A simple python script that gets query a list of site import requests
import time
import multiprocessing

def get_single_site(url):
    with requests.get(url) as response:
        print(f"Read {len(response.content)} from {url}")

def get_all_sites(sites):
    with multiprocessing.Pool(5) as pool:
        pool.map(get_single_site, sites)

if __name__ == "__main__":
    start_time = time.time() # our scripts start time
    sites = [
        "https://www.google.com",
        "https://www.facebook.com",
        "https://www.twitter.com/theghostyced"
    ] * 30

    get_all_sites(sites)
    end_time = time.time() - start_time

    print(f"Downloaded {len(sites)} in {end_time} seconds") 
```

这里我们从 Python 的标准库中导入多处理包。多处理模块附带了一些子类进程和池。

这里我们使用了`Pool`子类。`Pool`将它需要产生的工人或进程的数量作为它的第一个参数，这就是发生在`with multiprocessing.Pool(5) as pool:`行上的事情。在`pool.map(get_single_site, sites)`上，我们使用提供给池的 map 方法。该方法将需要调用的函数作为第一个参数，将我们的 URL 列表 iterable 作为第二个参数。然后，它将 iterable 分割成许多块，作为单独的任务提交给进程池。

给定操作的输出是:-

```
...
Read 608423 from https://www.twitter.com/theghostyced
Read 108078 from https://www.facebook.com
Read 11386 from https://www.google.com
Read 11387 from https://www.google.com
Read 11304 from https://www.google.com
Read 11353 from https://www.google.com
Read 108021 from https://www.facebook.com
Read 107985 from https://www.facebook.com
Read 108022 from https://www.facebook.com
Read 608423 from https://www.twitter.com/theghostyced
Read 108079 from https://www.facebook.com
Read 608423 from https://www.twitter.com/theghostyced
Read 11340 from https://www.google.com
Read 608423 from https://www.twitter.com/theghostyced
Read 11321 from https://www.google.com
Read 608423 from https://www.twitter.com/theghostyced
Read 107985 from https://www.facebook.com
Read 608423 from https://www.twitter.com/theghostyced
Read 11384 from https://www.google.com
Read 107549 from https://www.facebook.com
Read 608423 from https://www.twitter.com/theghostyced
Read 11294 from https://www.google.com
Read 608423 from https://www.twitter.com/theghostyced
Read 107985 from https://www.facebook.com
Read 11360 from https://www.google.com
Read 609124 from https://www.twitter.com/theghostyced
Downloaded 90 in 6.056399154663086 seconds 
```

这里，脚本花了 6 秒钟完成任务，比线程解决方案略快。这是合理的，因为正在进行的操作是 I/O 有界的。多重处理在处理 CPU 密集型操作时表现更好，比如处理大量数据。

## 结论

我知道您现在很想亲自尝试一下，所以在您走之前，我想告诉您一些关于何时使用并发的注意事项。

你需要先弄清楚你的程序是受 CPU 限制还是受 I/O 限制。请记住，I/O 绑定的程序是那些花费大部分时间等待事情发生(进行外部调用或请求)的程序，而 CPU 绑定的程序花费时间尽可能快地处理数据或计算数字。

因此，对于 I/O 受限的操作，多线程将是最好的方法，而对于 CPU 受限的操作，多处理将是正确的方法。

* * *

👋 👋 👋 👋我是 [CED](https://twitter.com/theghostyced) ，我希望你喜欢这个关于如何加速 Python 运行时间的教程🐍剧本。