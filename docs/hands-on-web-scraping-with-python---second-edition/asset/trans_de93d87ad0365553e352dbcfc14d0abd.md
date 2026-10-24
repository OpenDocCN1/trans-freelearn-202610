

这里，我们尝试在具有 name 属性且值分别为 keywords 和 description 的 <meta> 标签的 content 属性中查找信息：

```
source.find('meta[name="keywords"]').attr('content')
# web scraping, Web Scraper, Chrome extension, Crawling,
  Cross platform scraper
source.find('meta[name="description"]').attr('content')
# You need to train your web scraper? We have created
  simple test sites that allow you to try all corner cases
  and proof test your scraper. Try it now.
```

以下代码演示了 html() 函数的使用。它返回所提供表达式的 HTML 内容：

```
source.find('ul.dropdown-menu').html()
# \n\t...\t<li>\n\t...\t<a href="/documentation">Documentation</a>\n\t...\t</li>\n\t...\t<li>\n\t...\t<a href="/tutorials">Video Tutorials</a>\n\t...\t</li>\n\t...\t<li>\n\t...\t<a href="/how-to-videos">How to</a>\n\t...\t</li>\n\t...\t<li>\n\t...\t<a href="/test-sites">Test Sites</a>\n\t...\t</li>\n\t...\t<li>\n\t...\t<a href="https://forum.webscraper.io/" target="_blank" rel="noopener">Forum</a>\n\t...\t</li>\n\t\t\t\t\t
```

这种技术对于在不使用 DevTools 元素选择器或用于验证 DevTools 的输出的情况下识别内部或子元素及其属性和内容非常方便。

> **重要说明**
> 类和 id CSS 属性分别用 . 和 # 表示。例如，<a class="main" id="mainLink"> 将表示为 a.main 和 a#mainLink。

PyQuery 还包含与表达式一起使用的伪类或伪元素。这些元素以 : 字符开头，例如 :eq()、:last 和 :first。它们就像实现某些快捷方式的预定义常量。大多数情况下，伪元素与已识别的可迭代（集合）类型的元素一起使用。我们将在 Iterating using PyQuery 部分更详细地介绍迭代。

以下代码块中显示了 PyQuery 伪元素的使用。这些元素类似于快捷方式，并且大多数情况下消除了迭代的需要：

```
source.find('a:eq(0)').text()    # Toggle navigation
source.find('a.menuitm:first').text() # Web Scraper
source.find('a.menuitm:last').text()    # Learn
source.find('a.menuitm:eq(1)').attr('href')
# /cloud-scraper
```

让我们讨论一下在前面的代码中使用的一些伪元素：

- `:eq()`: 接受一个索引号（在 Python 中索引从 0 开始）。它评估匹配的表达式并收集与其关联的文本。这也可以用作 `source.find('a')`. `eq(0).text()`. 还有一些元素与 `:eq()` 类似：
  - `:lt()`: 接受一个索引号并且仅返回 *小于* 所提供索引号的值，例如，`source.find('a:lt(2)')`
  - `:gt()`: 接受一个索引号并且仅返回 *大于* 所提供索引号的值，例如，`source.find('a:gt(0)')`
- `:first`: 返回匹配表达式的元素的 *第一个出现*，与 `:eq(0)` 相同。
- `:last`: 返回匹配表达式的元素的 *最后一个出现*。

还有一些 *伪元素*，不过它们与前一个代码块中列出的有所不同：

```
source.find(':input')
# [<button.navbar-toggle.pull-right.collapsed>]

source.find(':header')
# [<h1>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>]

source.find(':empty')
# [<meta>, <meta>, <meta>, <meta>, <link>, <meta>, <link>, <link>, <link>, <script>, <iframe>, <span.icon-bar.top-bar>, <span.icon-bar.middle-bar>, <span.icon-bar.bottom-bar>, <img>, <a>, … <hr.test-site-divider>, <img>, <hr.test-site-divider>, <img>… <div.clearfix>, <div.push>, <br>, <img>, <img>]

source.find('meta:empty')
# [<meta>, <meta>, <meta>, <meta>, <meta>]

source.find(':empty:even')
# [<meta>, <meta>, <meta>, <link>, <link>, <iframe>, <span.icon-bar.middle-bar>, <img>, <div.crta>, <div.crta>, <hr.test-site-divider>, <hr.test-site-divider>, <hr.test-site-divider>, <hr.test-site-divider>, <hr.test-site-divider>, <div.clearfix>, <br>, <img>]

source.find(':header:odd')
# [<h2.site-heading>, <h2.site-heading>, <h2.site-heading>, <h2.site-heading>]
source.find('meta:empty:odd')   # [<meta>, <meta>]
```

让我们探索一下在前面的代码中使用的 *伪元素*：

- `:input`: 返回所有 <form> 输入元素或接收某些操作的元素，例如 <button>。`source(' :input ' )` 等同于 `source.find(' :input ')`。HTML 中还有 <input> 元素，可以表示为 `source.find('input')`。
- `:header`: 返回找到的 HTML 标题元素（<h1>、<h2>... <h6>）。
- `:empty`: 返回所有没有子元素的元素。这有助于在继续迭代内部元素之前进行检查。
- `:even`: 返回所有偶数索引的元素。它们也可以与其他 *伪元素* 一起使用。这里有两个示例：`source.find('meta:empty:even')` 和 `source('meta;empty:even')`。
- `:odd`: 与 `:even` 类似，但返回奇数索引的元素。

除了索引相关任务和查找元素之外，*伪元素* 还可用于搜索具有所提供文本的元素：

```
source.find('a:contains("Web")')
# [<a.menuitm>, <a>, <a>, <a>, <a>]

source.find('a:contains("Web"):last').text()
# 'Web Scraper'
source.find('a:contains("Web"):last').attr('href') # '# '
```

如前面的代码所示，`:contains()` 接受一个字符串参数，该参数区分大小写并返回匹配的元素。

PyQuery 还具有一些验证函数。这些函数在许多情况下非常有效（例如更新现有爬虫），用于搜索具有属性的元素并确认属性的值。

图 4.2 显示了 https://webscraper.io/test-sites 的页面源代码块，其中包含 <p class="copyright">：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_112_0.png)

图 4.2：页面源视图

在前面的代码中，source.find('a.menuitm, a[class*="btn-menu"]').items() 收集与表达式 a.menuitm 和 a[class*="btn-menu"] 匹配的元素。遍历它们找到了 items() 的 href 属性；我们总共有六个结果。

我们在 Element traversing, attributes, and pseudo-classes 部分遇到了许多 <meta> 标签。让我们收集常见 <meta> 标签的 name 和 content 属性的值（如果存在）：

```
meta=[]
count=0
for item in source.find('meta').items():
    if item.attr('name') and item.attr('content'):
        meta.append({'index':count,'name':item.attr('name')
            , 'content':item.attr('content')})
        count+=1
```

前面的代码遍历 source.find('meta').items()。对于每个 <meta> 标签或项目，已应用逻辑步骤来检查 name 和 content 属性是否存在。如果存在，它们将作为 Python 字典或 dict() 对象附加到 meta 输出列表。最终的 meta 输出列表如下所示：

```
[ {'index': 2,  'name': 'keywords',  'content': 'web scraping,Web Scraper,Chrome extension,Crawling,Cross platform scraper'},
{'index': 3,  'name': 'description',  'content': 'You need to train your web scraper? We have created simple test sites that allow you to try all corner cases and proof test your scraper. Try it now.'},
{'index': 4,  'name': 'viewport',  'content': 'width=device-width, initial-scale=1.0'}]
```

前面的代码中的输出提供了以下信息：

- 总共有五个 <meta> 标签（索引 0 到 4）
- 在五个 <meta> 标签中，第一和第二个标签要么没有 name 和 content 属性，要么缺少其中一个

在本节中，我们学习了如何部署 PyQuery 功能以获得特定结果，并测试了迭代和伪类等技术。在下一节中，我们将学习如何实现前面章节和前几节中的所有概念来从网络抓取数据。

## 使用 PyQuery 进行网络抓取

本节包含大量示例，解释如何编写网络抓取脚本。我们将使用 PyQuery 和 requests 库并处理来自网络的 HTML 和 XML 内容。我们将把这些示例中收集的数据写入 CSV 或 JSON 文件。

