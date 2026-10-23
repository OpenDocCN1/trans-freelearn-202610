

我们可以将不同的格式化器作为参数传递给任何输出方法，例如 `prettify()`、`encode()` 和 `decode()`。

## 最小格式化器

在此格式化模式下，字符串将被处理到足以生成有效的 HTML 代码。这是默认的格式化器，被转义的 HTML 实体是 `&amp;`、`&gt;` 和 `&lt;`。输出将类似于上一张截图中所示的内容。

## html 格式化器

在此格式化模式下，Beautiful Soup 将把 Unicode 字符转换为 HTML 实体。

```
print(soup.prettify(formatter="html"))
```

以下截图显示了使用 `minimal` 和 `html` 格式化器的区别：

![](img/c437561ffd11fadb571896e0a4ca4d18_108_0.png)

从上一张截图我们可以看出，格式化器 `html` 会将尽可能多的 Unicode 字符转换回 HTML 实体。

## None 格式化器

在这种情况下，Beautiful Soup 不会修改字符串。这可能导致生成无效的 HTML 代码。

`print(soup.prettify(formatter=None))` 代码行产生的输出类似于以下截图：

![](img/c437561ffd11fadb571896e0a4ca4d18_109_0.png)

## 函数格式化器

我们可以指定一个 Python 函数来处理每个字符串和属性值。

```
def remove_chara(markup):
    return markup.replace("a", "")
```

这里我们定义一个函数，从传递的字符串中删除字符 a。

```
print(soup.prettify(formatter=remove_chara))
```

我们使用此函数从输出中删除 a 字符。需要注意的是，这不会转义输出中的三个特殊字符 &amp;、&gt; 和 &lt;，如以下截图所示：

![](img/c437561ffd11fadb571896e0a4ca4d18_109_1.png)

## 使用 get_text()

从网站获取纯文本是常见的任务。Beautiful Soup 为此提供了 `get_text()` 方法。

如果我们只想获取 Beautiful Soup 或 Tag 对象的文本，可以使用 `get_text()` 方法。例如：

```
html_markup = """<p class="ecopyramid">
<ul id="producers">
    <li class="producerlist">
        <div class="name">plants</div>
        <div class="number">100000</div>
    </li>
    <li class="producerlist">
        <div class="name">algae</div>
        <div class="number">100000</div>
    </li>
</ul>"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.get_text())

#output
plants
100000

algae
100000
```

`get_text()` 方法以单个 Unicode 字符串的形式返回 Beautiful Soup 或 Tag 对象内部的文本。但 `get_text()` 在处理网页时存在问题。网页通常包含 JavaScript 代码，而 `get_text()` 方法也会返回 JavaScript 代码。例如，在第 3 章《使用 Beautiful Soup 搜索》中，我们看到了从 packtpub.com 抓取图书详情的示例。

```
import urllib2
from bs4 import BeautifulSoup
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page,"lxml")
```

我们可以使用 `get_text()` 打印页面内的文本；这在以下代码片段中显示：

```
print(soup_packtpage.get_text())
```

使用上一行代码，我们也会在输出中得到 JavaScript 代码，如下所示：

```
$(window).load(function() {
  $("img[data-original]").addClass("lazy");
  $("img.lazy").lazyload();
  setTimeout(function() {
    addthis_config = Drupal.settings.addthis.config_default;
    addthis_share = Drupal.settings.addthis.share_default;
    if (typeof pageTracker != "undefined")
    {addthis_config.data_ga_tracker = pageTracker;}
    var at = document.createElement("script"); at.type =
      "text/javascript"; at.async = true;
    at.src = "//dgdsbygo8mp3h.cloudfront.net/sites/default/
      files/addthis/addthis_widget.js";
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(at, s);
  }, 5);
});
```

删除 JavaScript 并仅打印文档中的文本，可以通过以下代码行实现：

```
[x.extract() for x in soup_packtpage.find_all('script')]
```

上一行代码将从文档中删除所有 script 元素。之后，`print(soup_packtpage.get_text())` 代码行将仅打印页面中存储的文本。

## 快速参考

您可以查看以下参考资料以获得概览：

- 格式化打印：以下方法给出格式化输出：
  - soup.prettify(): 此方法用于带缩进和格式的输出
- 非格式化打印：这些方法给出不带任何格式或缩进的输出：
  - str(soup)
  - soup.encode()
  - soup.decode()
- 格式化器：这些方法能够控制输出的行为：
  - `soup.prettify(formatter="minimal")`
  - `soup.prettify(formatter="html")`
  - `soup.prettify(formatter="None")`
  - `soup.prettify(formatter=already_defined_func)`
- get_text(): 此方法为您提供对象或标签的所有内容：
  - `soup.get_text()`: 此方法用于获取 BeautifulSoup 或 Tag 对象中的所有文本内容

## 总结

本章介绍了 Beautiful Soup 中的不同输出方法。我们看到了 BeautifulSoup 对象中的格式化和非格式化打印，以及用于控制输出格式的不同输出格式化器。我们还看到了从网页中仅获取文本的方法。

在下一章中，我们将使用本書迄今为止所研究的搜索、导航和其他技术创建一个网络抓取器。

## 8

## 创建网络抓取器

在本章中，我们将使用前几章研究的不同的搜索和导航技术创建一个网站抓取器。抓取器将访问三个网站，根据 ISBN 查找图书的售价。我们将首先从 packtpub.com 找到图书列表和售价。我们还将从 packtpub.com 找到图书的 ISBN，并基于此 ISBN 在 www.amazon.com 和 www.barnesandnoble.com 等其他网站上进行搜索。通过这样做，我们将自动化在三个网站上查找图书售价的过程，并将获得为这些网站实现抓取器的实践经验。由于网站结构可能会在以后发生变化，本章中使用的代码示例和图像也可能变得无效。因此，最好将示例作为参考并相应地更改代码。访问这些网站以获得更好的理解是很好的。

## 从 PacktPub.com 获取图书详情

从 www.packtpub.com 获取图书详情是创建抓取器的第一步。我们需要从 PacktPub.com 找到以下详情：

- 书名
- 售价
- ISBN

我们在第 3 章《使用 Beautiful Soup 搜索》中看到了如何从 packtpub.com 抓取书名和售价。该章中讨论的示例仅考虑了第一页，未包含也包含图书列表的其他页面。因此，在下一个主题中，我们将找到包含图书列表的不同页面。

## 查找包含图书列表的页面

www.packtpub.com/books 页面上有下一个和上一个导航链接，用于在包含图书列表的页面之间前后移动，如以下截图所示：

![](img/c437561ffd11fadb571896e0a4ca4d18_114_0.png)

因此，我们需要找到一种获取包含图书列表的多个页面的方法。逻辑上，似乎要查看当前页面中 next 元素指向的页面。使用 Google Chrome 开发工具查看 next 元素，对于第 49 页，我们可以看到它实际上指向下一页链接，即 /books?page=49。如果我们使用 Google Chrome 开发工具观察不同的页面，可以看到下一页的链接实际上具有 /books?page=n 的模式，对于第 n+1 页，即 n=49 表示第 50 页，如以下截图所示：

![](img/c437561ffd11fadb571896e0a4ca4d18_114_1.png)

从前面的截图中，我们可以进一步理解 next 元素位于 class="pager-next last" 的 <li> 标签内。在 <li> 标签内，有一个 <a> 标签保存着指向下一页的链接。在这种情况下，对应的值为 /books?page=49，指向第 50 页。我们必须将 www.packtpub.com 添加到此值以构成有效的 URL，即 www.packtpub.com/books?page=49。

