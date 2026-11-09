
看起来结果很不错。你可能会注意到，第三列的文本周围没有封装字符。这可能会成为一个问题，因为我们使用了逗号作为分隔符。如果第三列中出现逗号怎么办？如果这让你担心，你可以给第三列加上引号，或者使用制表符来分隔列。要做到这一点，将第一个 `outfile.write()` 行改为使用 \t（制表符）作为连接字符而不是逗号。你也可以通过 `lstrip()` 对消息进行一些空白修剪，以去除任何多余字符。

## 使用正则表达式解析 HTML 的局限性

这种正则表达式方法乍一看似乎很直接，但它有一些局限性。首先，对于新手数据清洗人员来说，设计和完善正则表达式可能有点让人头疼。你应该做好花大量时间调试并为自己编写大量文档的准备。为了帮助生成正则表达式，我强烈建议使用正则表达式测试工具，例如 http://Pythex.org，或者直接使用你喜欢的搜索引擎查找一个。确保你指定想要 Python 正则表达式测试工具，如果你使用的语言是 Python。

接下来，你应该事先知道，正则表达式完全依赖于网页结构在未来保持不变。因此，如果你计划按计划从网站收集数据，今天编写的正则表达式明天可能就无法工作。它们只有在页面布局不发生变化时才有效。两个标签之间添加一个空格就会导致整个正则表达式失败，并且极难排查问题。还要记住，大多数时候你对网站更改几乎没有控制权或完全没有控制权，因为你收集数据的通常不是你自己的网站！

最后，有很多很多情况，准确编写正则表达式来匹配给定的 HTML 结构几乎是不可能的。Regex 强大但不完美也不万无一失。关于这个问题的一个有趣看法，我推荐你阅读在 Stack Overflow 上被点赞超过 4000 次的著名回答：http://stackoverflow.com/questions/1732348/。在该回答中，作者幽默地表达了许多程序员反复尝试解释为什么正则表达式不是解析网页中不规则且不断变化的 HTML 的完美解决方案时的挫败感。

## 方法二 – Python 和 BeautifulSoup

由于正则表达式存在一些局限性，我们肯定需要在数据清洗工具包中增加更多工具。在这里，我们描述如何使用名为 BeautifulSoup 的基于解析树的 Python 库从 HTML 页面中提取数据。

### 第一步 – 查找并保存用于实验的文件

对于这一步，我们将使用与方法 1 相同的文件：来自 Django IRC 频道的文件。我们将搜索相同的三个项目。这样做可以轻松比较两种方法。

### 第二步 – 安装 BeautifulSoup

BeautifulSoup 目前是第 4 版。此版本适用于 Python 2.7 和 Python 3。

> 如果你使用的是 Enthought Canopy Python 环境，只需在 Canopy 终端中运行 `pip install beautifulsoup4` 即可。

### 第三步 – 编写 Python 程序来提取数据

我们感兴趣的三个项目都存在于 `li` 标签集合中，具体来说是那些具有 `class="le"` 的标签。在这个特定文件中没有其他 `li` 标签，但为了以防万一，我们还是具体一点。以下是我们想要的项目以及在解析树中的查找位置：

- 我们可以从 `li` 标签下的 `rel` 属性中提取用户名。
- 我们可以从 `a` 标签中的 `name` 属性获取 `linenum` 值。`a` 标签也是 `li` 标签的内容中的第一项。

> 请记住数组是从零开始的，所以我们需要请求第 0 项。

在 BeautifulSoup 中，标签的 **内容** 是解析树中该标签下方的项目。其他一些包会称它们为 **子** 项。

- 我们可以将消息提取为 li 标签中的第四个内容项（引用为数组项 [3]）。我们还注意到每个消息前面都有一个前导空格，所以在保存数据之前我们想将其去掉。

以下是对应于我们想要从解析树中获取内容的轮廓的 Python 代码：

```
from bs4 import BeautifulSoup
import io

infile = io.open('django13-sept-2014.html', 'r', encoding='utf8')
outfile = io.open('django13-sept-2014.csv', 'a+', encoding='utf8')
soup = BeautifulSoup(infile)

row = []
allLines = soup.findAll("li","le")
for line in allLines:
    username = line['rel']
    linenum = line.contents[0]['name']
    message = line.contents[3].lstrip()
    row.append(linenum)
    row.append(username)
    row.append(message)
    outfile.write(', '.join(row))
    outfile.write(u'\n')
    row = []
infile.close()
```

### 第四步 – 查看文件并确保其干净

当我们在文本编辑器中打开新的 CSV 文件时，可以看到前几行现在看起来与方法 1 中的完全相同：

```
1574618, petisnnake, i didn't know that
1574619, dshap, FunkyBob: ahh, hmm, i wonder if there's a way to do
it in my QuerySet subclass so i'm not creating a new manager subclass
*only* for get_queryset to do the initial filtering
1574620, petisnnake, haven't used Django since 1.5
```

就像使用正则表达式方法一样，如果你担心最后一列中嵌入了逗号，你可以对列文本进行封装引号，或者直接使用制表符来分隔列。

## 方法三 – Chrome Scraper

如果你真的不想编写程序来解析数据，有几个基于浏览器的工具使用树结构让你能够识别和提取你感兴趣的数据。我认为最容易使用且工作量最小的是一个名为 Scraper 的 Chrome 扩展，由一名名为 mnmldave 的开发者创建（真名：Dave Heaton）。

### 第一步 – 安装 Scraper Chrome 扩展

如果尚未运行，请下载 Chrome 浏览器。确保你获取的是正确的 Scraper 扩展；有几个扩展名称非常相似。我建议使用该产品的开发者自己的 GitHub 网站，网址为 http://mnmldave.github.io/scraper/。这样你就可以获得正确的抓取工具，而不是尝试使用 Chrome 商店搜索。从 http://mnmldave.github.io/scraper 网站，点击链接从 Google 商店安装扩展，然后重新启动浏览器。

### 第二步 – 从网站收集数据

将浏览器指向我们用于另外两次网页数据提取实验的相同网页 URL，即 Django IRC 日志之一。我在这里的示例和屏幕截图中使用的是 2014 年 9 月 13 日的日志，所以我将访问 http://django-irc-logs.com/2014/sep/13/。

在我撰写本文时，我的浏览器中该页面看起来如下：

![](img/9aa003afabe05fdb07da46ad71325de6_139_0.png)

我们对这个 IRC 日志感兴趣的三个项目是：

- 行号（我们从之前两次实验中知道这是 # 符号下链接的一部分）
- 用户名（位于 < 和 > 符号之间）
- 实际行消息

Scraper 允许我们依次突出显示这三个项目中的每一个，并将值导出到 Google 电子表格中，然后我们可以将它们重新组装到一个工作表中并导出为 CSV（或随心所欲地处理它们）。操作方法如下：

1. 使用鼠标突出显示你想要抓取的项目。
2. 右键单击并从菜单中选择 Scrape similar...。在下面的示例中，我选择了用户名 petisnnake 作为我希望 Scraper 使用的用户名：

![](img/9aa003afabe05fdb07da46ad71325de6_140_0.png)

3. 选择 Scrape similar 后，工具会显示一个新窗口，其中包含页面上所有相似项目的集合。下面的屏幕截图显示了 Scraper 找到的整个用户名列表：
