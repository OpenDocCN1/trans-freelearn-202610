

```html
<div class="number">100</div>

</li>

</ul>
<ul id="tertiaryconsumers">

<li class="tertiaryconsumerlist">

<div class="name">lion</div>

<div class="number">80</div>

</li>

<li class="tertiaryconsumerlist">

<div class="name">tiger</div>

<div class="number">50</div>

</li>

</ul>
```

所以，我们找到生产者的下一个兄弟节点，它们是初级消费者、次级消费者和三级消费者。

我们可以使用 `find_previous_siblings()` 和 `find_previous_sibling()` 分别查找之前的兄弟节点和之前的兄弟节点。

与其他查找方法一样，我们可以为这些方法传递不同的过滤器，例如文本、正则表达式、属性值和标签名，以便相应地查找兄弟节点。

## 搜索下一个

对于每个标签，都存在一个下一个元素，它可以是可导航字符串、Tag 对象或任何其他 BeautifulSoup 对象。 所谓下一个元素，是指在当前元素之后立即解析的元素。 这与兄弟节点不同。 我们有方法可以为特定 Tag 对象查找下一个对象。 `find_all_next()` 方法有助于找到标签之后出现的所有对象，而 `find_next()` 找到在 Tag 对象之后出现的第一个对象。 在此方法中，我们也可以像在 `find()` 中一样使用不同的搜索方法。

例如，我们可以使用以下代码找到第一个 `div` 标签之后出现的所有 `li` 标签：

```python
first_div = soup.div
all_li_tags = first_div.find_all_next("li")

#output
[<li class="producerlist">
    <div class="name">plants</div>
    <div class="number">100000</div>
</li>, <li class="producerlist">
    <div class="name">algae</div>
    <div class="number">100000</div>
</li>, <li class="primaryconsumerlist">
    <div class="name">deer</div>
    <div class="number">1000</div>
</li>, <li class="primaryconsumerlist">
    <div class="name">rabbit</div>
    <div class="number">2000</div>
</li>, <li class="secondaryconsumerlist">
    <div class="name">fox</div>
    <div class="number">100</div>
</li>, <li class="secondaryconsumerlist">
    <div class="name">bear</div>
    <div class="number">100</div>
</li>, <li class="tertiaryconsumerlist">
    <div class="name">lion</div>
    <div class="number">80</div>
</li>, <li class="tertiaryconsumerlist">
    <div class="name">tiger</div>
    <div class="number">50</div>
</li>]
```

## 搜索上一个

搜索上一个是下一个的相反情况，我们可以在特定对象关联的上下文中找到上一个对象。 我们可以使用 `find_all_previous()` 方法找到与当前对象关联的所有上一个对象，并使用 `find_previous()` 找到与当前对象关联的上一个对象。

## 使用搜索方法从网页抓取信息

在 *第 2 章，创建 BeautifulSoup 对象* 中，我们讨论了获取 Packt Publishing 出版的所有书籍的信息，这些书籍可在链接 `http://www.packtpub.com/books` 获取。 在以下练习中，我们将使用 Beautiful Soup 搜索方法抓取前述 URL 的所有书籍。 我们将从前述 URL 抓取书籍名称、出版日期和价格。

此练习的第一步是分析 HTML 文档并理解其逻辑结构。 这将帮助我们理解所需信息如何在 HTML 文档中存储。

我们可以使用 Google Chrome 开发者工具来理解页面的逻辑结构。 让我们通过执行以下步骤来完成此操作：

1. 在 Google Chrome 浏览器中打开页面。
2. 右键单击它并选择 **Inspect Element** 选项。
3. 现在，我们可以在页面底部看到 **Developer tools** 窗口。

在 Google Chrome 开发者工具下，前述 URL 的内容将如下所示，针对特定书籍标题：

![](img/c437561ffd11fadb571896e0a4ca4d18_56_0.png)

前述屏幕截图是在撰写本书时前述 URL 的表示。 由于这是一个在线页面，数据将定期更新。

我们可以看到每个书籍标题都存储在具有 `class="field-content"` 的 `<span>` 标签下。 如果仔细观察，我们可以看到出版日期信息和价格信息也存储在具有相同 `class="field-content"` 的类似 `<span>` 标签内。 为了唯一标识前述数据，我们不能仅取 `<span>` 标签，因为三条信息都存储在具有相同类的 `<span>` 标签下。 因此，更好地识别每个信息唯一包含的另一个标签。 在这种情况下，我们可以看到对应于书籍标题的 `<span>` 标签存储在具有 `class="views-field-title"` 的 `<div>` 标签内。 同样，对于出版日期信息，对应的 `<span>` 标签存储在具有 `class="views-field-field-date-of-publication-value"` 的 `<div>` 标签内。 同样，价格的 `<span>` 标签存储在具有 `class="views-field-sell-price"` 的 `<div>` 标签内。 从网页源代码中，我们可以看到结构如下，并且支持我们之前的发现：

```html
<div class="views-field-title">
    <span class="field-content">
        <a href="/angularjs-web-application-development/book">
            Mastering Web Application Development with AngularJS</a>
    </span>
</div>
<div class="views-field-field-date-of-publication-value">
    <span class="field-content">Published: August 2013</span>
</div>
<div class="views-field-sell-price">
    <label class="views-label-sell-price">
        Our price:
    </label>
    <span class="field-content">
        1,386.00</span>
</div>
```

4. 要找到对应标题的价格，我们应该搜索下一个具有 `class="views-field-sell-price"` 的 `<div>` 标签。 从逻辑结构中，我们可以看到对于出版日期，我们也应该获取 `<span>` 标签内的字符串。

我们可以对页面内的所有标题应用前述解析策略。 下一步是将此转换为脚本，以便我们从页面获取与书籍标题相关的所有信息。

我们将首先根据上一章的示例，通过以下方式基于网页创建 `BeautifulSoup` 对象：

```python
import urllib2
from bs4 import BeautifulSoup
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page)
page.close()
```

首先，我们将搜索具有 `class="views-view-grid"` 的表格，如下所示：

```python
all_books_table = soup_packtpage.find("table", class_="views-view-grid")
```

然后，我们将在此表格标签内搜索所有具有 `views-field-title` 类的 `div` 标签，如下所示：

```python
all_book_titles = all_books_table.find_all("div", class_="views-field-title")
```

作为下一步，我们将遍历这些标签对象，它们代表书籍标题，并为每个标题找到出版日期和价格，如下所示：

```python
for book_title in all_book_titles:
    book_title_span = book_title.span
    print("Title Name is :"+book_title_span.a.string)
    published_date = book_title.find_next("div", class_="views-field-field-date-of-publication-value")
    print("Published Date is :"+published_date.span.string)
    price = book_title.find_next("div", class_="views-field-sell-price")
    print("Price is :"+price.span.string)

#output
Title Name is :Mastering Web Application Development with AngularJS
```

## 快速参考

在这里，我们将看到 Beautiful Soup 中的以下搜索方法：

- `find()`: 此函数用于查找第一次出现
- `find('p')`: 此函数用于查找标签的第一次出现
- `find(text="newtext")`: 此函数用于查找文本的第一次出现
- `find(attrs={'id':'value'})`: 此函数用于根据属性值查找属性的第一次出现
- `find(class_='value')`: 此函数用于查找具有 CSS 类值的第一次出现
- `find_all()`: 此函数用于根据过滤条件查找所有出现
- `find_parent()`: 此函数用于在父标签中查找第一次出现
- `find_parents()`: 此函数用于在父标签中查找所有出现
- `find_sibling()`: 此函数用于在兄弟标签中查找第一次出现
- `find_siblings()`: 此函数用于在兄弟标签中查找所有出现
- `find_next()`: 此函数用于查找在当前标签之后立即解析的标签的第一次出现
- `find_all_next()`: 此函数用于查找在当前标签之后立即解析的所有标签的出现
- `find_previous()`: 此函数用于查找在当前标签之前立即解析的标签的第一次出现
- `find_all_previous()`: 此函数用于查找在当前标签之前立即解析的所有标签的出现

