

## 使用 Beautiful Soup 进行搜索

```
```text
#output
lion
tiger
```
```

## 了解 find_all() 使用的参数

与 `find()` 类似，`find_all()` 方法也有一组相似的参数，并额外增加了一个参数 `limit`，如下代码行所示：

```
```python
find_all(name,attrs,recursive,text,limit,**kwargs)
```
```

`limit` 参数用于指定我们获取的结果数量上限。例如，从我们看到的电子邮件 ID 示例中，我们可以使用 `find_all()` 获取所有电子邮件 ID。请参考以下代码：

```
```python
email_ids = soup.find_all(text=emailid_regexp)
print(email_ids)

#output
[u'abc@example.com',u'xyz@example.com',u'foo@example.com']
```
```

这里，如果我们传入 `limit`，它会将结果集限制为我们设定的上限，如以下示例所示：

```
```python
email_ids_limited = soup.find_all(text=emailid_regexp,limit=2)
print(email_ids_limited)

#output
[u'abc@example.com',u'xyz@example.com']
```
```

从输出中可以看出，结果被限制为两个。

> > 💡 `find()` 方法相当于 `find_all()` 且 `limit=1`。

我们可以向查找方法传递 True 或 False 值。如果向 `find_all()` 传递 True，它将返回 soup 对象中的所有标签。对于 `find()` 来说，它将是对象内的第一个标签。`print(soup.find_all(True))` 代码行将打印出与 soup 对象关联的所有标签。

在搜索文本时，传递 True 将返回文档中的所有文本，如下所示：

```
all_texts = soup.find_all(text=True)
print(all_texts)

#output
[u'\n', u'\n', u'\n', u'\n', u'\n', u'plants', u'\n', u'100000',
    u'\n', u'\n', u'\n', u'algae', u'\n', u'100000', u'\n', u'\n',
    u'\n', u'\n', u'\n', u'deer', u'\n', u'1000', u'\n', u'\n',
    u'\n', u'rabbit', u'\n', u'2000', u'\n', u'\n', u'\n',
    u'\n', u'\n', u'fox', u'\n', u'100', u'\n', u'\n', u'\n', u'\n',
    u'bear', u'\n', u'100', u'\n', u'\n', u'\n', u'\n', u'\n',
    u'\n', u'lion', u'\n', u'80', u'\n', u'\n', u'\n',
    u'tiger', u'\n', u'50', u'\n', u'\n', u'\n', u'\n',
    u'\n']
```

前面的输出打印了 soup 对象内的所有文本内容，包括换行符。

此外，在文本的情况下，我们可以传递一个字符串列表，find_all() 将查找列表中定义的每个字符串：

```
all_texts_in_list = soup.find_all(text=["plants","algae"])
print(all_texts_in_list)

#output
[u'plants', u'algae']
```

在搜索标签、标签属性值、自定义属性和 css 类时也是如此。

要查找所有 div 和 li 标签，我们可以使用以下代码行：

```
div_li_tags = soup.find_all(["div","li"])
```

同样，要查找具有 producerlist 和 primaryconsumerlist 类的标签，我们可以使用以下代码行：

```
all_css_class =
    soup.find_all(class_=["producerlist","primaryconsumerlist"])
```

`find()` 和 `find_all()` 都搜索对象的后代（即在树中在其之后出现的所有子节点）、它们的子节点，等等。我们可以通过使用 recursive 参数来控制此行为。如果 recursive = False，搜索仅在对象的直接子节点上进行。

例如，在以下代码中，搜索仅在 div 和 li 标签的直接子节点处进行。由于 soup 对象的直接子节点是 html，以下代码将给出一个空列表：

```
div_li_tags = soup.find_all(["div","li"],recursive=False)
print(div_li_tags)

#output
[]
```

如果 find_all() 找不到结果，它将返回一个空列表，而 find() 返回 None。

## 按关系搜索标签

使用 find() 和 find_all() 方法在 HTML 页面中搜索内容很容易。在复杂的网络抓取项目中，用户很容易访问后续标签或父标签以获取额外信息。

我们打算访问的这些标签将与我们已搜索的标签直接相关。例如，我们可能需要找到特定标签的直接父标签。也有情况需要找到前一个标签、下一个标签、同级标签等。在这些情况下，BeautifulSoup 对象提供了搜索方法，例如 find_parents()、find_next_siblings() 等。通常，我们使用这些方法后跟 find() 或 find_all() 方法，因为这些方法用于查找特定标签，而我们有兴趣查找与该标签相关的其他标签。

## 搜索父标签

我们可以通过使用 find_parents() 或 find_parent() 方法找到与标签关联的父标签。同样，find_all() 和 find() 仅在返回结果数量上有所不同。find_parents() 方法返回所有匹配的父标签，而 find_parent() 返回第一个直接父标签。我们在 find() 中讨论的搜索方法可用于 find_parent() 和 find_parents()。

在初级消费者示例中，我们可以按如下方式找到与 primaryconsumer 关联的直接父标签：

```
primaryconsumers = soup.find_all(class_="primaryconsumerlist")
primaryconsumer = primaryconsumers[0]
parent_ul = primaryconsumer.find_parents('ul')
print(parent_ul)
```

```
#output
<ul id="primaryconsumers">

    <li class="primaryconsumerlist">

        <div class="name">deer</div>

        <div class="number">1000</div>

    </li>

    <li class="primaryconsumerlist">

        <div class="name">rabbit</div>

        <div class="number">2000</div>

    </li>

</ul>
```

第一行将把所有初级消费者存储在 primaryconsumers 变量中。我们取第一个条目并将其存储在 primaryconsumer 中。我们试图找到 <ul> 标签，它们是我们在 primaryconsumer 列表中找到的父标签。从前面的输出中，我们可以理解结果将包含父级的整个结构，这也将包括我们为其找到父级的标签。我们可以使用 find_parent() 方法找到标签的直接父级。

```
parent_p = primaryconsumer.find_parent("p")
```

前面的代码将搜索第一个 <p> 标签，它是生态金字塔示例中具有 primaryconsumerlist 类的 <li> 标签的直接父级。

获取特定标签的直接父标签的一种简便方法是使用不带任何参数的 find_parent() 方法，如下所示：

```
immediateprimary_consumer_parent = primary_consumer.find_parent()
```

结果将与 primary_consumer.find_parent('ul') 相同，因为 ul 是直接父标签。

## 搜索同级标签

在特定的 html 文档中，如果标签处于同一级别，我们可以说这些标签是同级标签。例如，在生态金字塔中，所有 <ul> 标签都在同一级别，如果我们定义关系，它们就是同级。我们可以从以下图表中理解这一点，该图表表示第一个 div 标签与 ul 标签之间的关系：

![](img/c437561ffd11fadb571896e0a4ca4d18_52_0.png)

这意味着在我们的示例中，具有 classes producers、primaryconsumers、secondaryconsumers 和 teritiaryconsumers 的 ul 标签是同级。

此外，在以下生产者的图表中，我们可以看到 plants 值，即第一个生产者，和 algae，即第二个生产者，不能被视为同级，因为它们不在同一级别：

![](img/c437561ffd11fadb571896e0a4ca4d18_52_1.png)

但是，具有值 plants 的 div 和值为 10000 的数字可以被视为同级，因为它们处于同一级别。

Beautiful Soup 提供了帮助我们查找同级的方法。

`find_next_siblings()` 方法允许查找下一个同级，而 `find_next_sibling()` 允许查找下一个同级。在以下示例中，我们可以找出 producers 的同级：

```
producers= soup.find(id='producers')
next_siblings = producers.find_next_siblings()
print(next_siblings)

#output
[<ul id="primaryconsumers">

    <li class="primaryconsumerlist">

        <div class="name">deer</div>

        <div class="number">1000</div>

    </li>

    <li class="primaryconsumerlist">

        <div class="name">rabbit</div>

        <div class="number">2000</div>

    </li>

</ul>, <ul id="secondaryconsumers">

    <li class="secondaryconsumerlist">

        <div class="name">fox</div>

        <div class="number">100</div>

    </li>

    <li class="secondaryconsumerlist">

        <div class="name">bear</div>
```

