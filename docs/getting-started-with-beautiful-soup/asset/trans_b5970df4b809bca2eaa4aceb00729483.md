

## 创建 BeautifulSoup 对象

我们可以在 BeautifulSoup 构造器中指定 features 参数，将输入视为 XML 进行处理，如下所示：

```
soup_xml = BeautifulSoup(helloworld,features="xml")
```

另一种替代方法是使用以下代码行：

```
soup_xml = BeautifulSoup(helloworld,"xml")
```

在之前的代码中，我们将 xml 作为 features 参数的值传递并创建了 soup_xml 对象。我们可以看到，相同的 content ("<p>HelloWorld</p>") 现在被当作 XML 而不是 HTML 来处理。

```
print(soup_xml)
```

```
#output
<?xml version="1.0" encoding="utf-8"?>
<p>Hello World</p>
```

在之前的示例中，Beautiful Soup 基于 lxml 解析器选择了 LXMLTreeBuilderForXML 并将输入解析为 XML。

> features 参数帮助我们选择文档的 HTML/XML 解析。

通过提供 features 参数，我们是在指定 TreeBuilder 应该具有的功能。如果 Beautiful Soup 无法找到具有给定功能的 TreeBuilder，则会抛出错误。例如，假设 lxml 是 Beautiful Soup 目前用于 XML 处理的唯一解析器，且未安装在系统中。在这种情况下，如果我们使用以下代码行：

```
soup_xml = BeautifulSoup(helloworld,features="xml")
```

之前的代码将失败并抛出以下错误（因为系统中未安装 lxml）：

```
bs4.FeatureNotFound: Couldn't find a tree builder with the
    features you requested: xml. Do you need to install a parser
    library?
```

在这种情况下，我们应该使用 easy_install、pip 或 setup.py install 安装所需的解析器。

在创建 BeautifulSoup 对象时始终最好指定要使用的解析器。这是因为不同的解析器会以不同的方式解析内容。这在我们提供无效 HTML 内容进行解析时更为明显。前面讨论的三个解析器在无效 HTML 的情况下会产生三种类型的 HTML 树。例如：

```
invalid_html = '<a invalid content'
```

这里的 HTML 无效，因为没有闭合的 </a> 标签。使用前面提到的解析器处理此无效 HTML 如下所示：

- 使用 lxml 解析器，如下所示：
  - soup_invalid_html = BeautifulSoup(invalid_html, 'lxml')
  - print(soup_invalid_html) 代码行将给出使用 lxml 解析器生成的 HTML 树。
  - <html><body><a invalid content=""></a></body></html>
  - 从输出中可以看出，lxml 解析器已处理了无效 HTML。它添加了闭合的 </a> 标签，并将无效内容视为 <a> 标签的属性。除此之外，它还添加了 <html> 和 <body> 标签，而这些标签在输入中并不存在。如果使用 lxml，默认情况下会添加 <html> 和 <body> 标签。
- 使用 html5lib 解析器，如下所示：
  - soup_invalid_html = BeautifulSoup(invalid_html, 'html5lib')
  - print(soup_invalid_html) 代码行将向我们展示使用 html5lib 解析器生成的 HTML 树。
  - <html><head></head><body></body></html>
  - 从输出中可以看出，html5lib 解析器添加了 <html>、<head> 和 <body> 标签，而这些标签在输入中并不存在。例如，lxml 解析器和 html5lib 解析器会对任何输入都添加这些标签。但与此同时，它已丢弃了无效的 <a> 标签以生成输入的不同表示。
- 使用 html.parser，如下所示：
  - soup_invalid_html =
    BeautifulSoup(invalid_html,'html.parser')

`print(soup_invalid_html)` 代码行将向我们展示使用 `html.parser` 生成的 HTML 树。

`html.parser` 丢弃了无效 HTML 并生成了空树。与其他解析器不同，它不会添加任何 `<html>`、`<head>` 或 `<body>` 标签。

因此，通过提供 `features` 参数来指定解析器是很好的，因为这有助于确保输入在不同机器上以相同的方式处理。否则，如果存在无效 HTML，相同的代码可能会在一台机器上中断，因为 Beautiful Soup 选择的默认解析器会生成不同的树。指定 `features` 参数有助于确保在所有机器上生成的树是相同的。

在创建 BeautifulSoup 对象时，还会创建其他对象，包括以下内容：

- Tag
- NavigableString

### Tag

Tag 对象表示 HTML 和 XML 文档的不同标签。Tag 对象的创建是在解析文档时完成的。解析期间识别的不同 HTML/XML 标签表示为相应的 Tag 对象，这些对象将具有 HTML/XML 标签的属性和内容。Tag 对象可用于在 HTML/XML 文档中进行搜索和导航。

### 从 BeautifulSoup 访问 Tag 对象

BeautifulSoup 允许我们访问任何 Tag 对象。例如，我们可以通过简单地调用标签名称 `<a>` 来访问下一个示例中 `<a>` 标签的第一次出现。

```
html_atag = """<html><body><p>Test html a tag example</p>
<a href='http://www.packtpub.com'>Home</a>
<a href='http://www.packtpub.com/books'>Books</a>
</body>
</html>"""
soup  = BeautifulSoup(html_atag,'lxml')
atag  = soup.a
print(atag)
```

之前的脚本将打印文档中的第一个 <a> 标签。我们可以看到 type(atag) 是 'bs4.element.Tag'。

HTML/XML 标签具有名称（例如，标签 <a> 的名称是 a，标签 <p> 是 p）和属性（例如，class、id 和 style）。Tag 对象允许我们获取与每个 HTML 标签关联的名称和属性。

### Tag 对象的名称

Tag 对象的名称可通过 .name 属性访问。

```
tagname = atag.name
print tagname
```

之前的代码打印了对象 atag 的名称，即标签 <a> 的名称。

我们可以通过更改 .name 属性的值来更改标签的名称。

这在以下示例中显示：

```
atag.name = 'p'
print(soup)

#output
<html><body><p>Test html a tag example</p>
<p href="http://www.packtpub.com">Home</p>
<a href="http://www.packtpub.com/books">Books</a>
</body></html>
```

从输出中，我们可以看到第一个 <a> 标签被替换为 <p> 标签。

### Tag 对象的属性

属性为标签提供含义和上下文。在之前的示例中，href 属性为 <a> 标签添加了 URL 信息。在 HTML 页面中，标签可能具有不同的属性，例如 class、id 和 style。标签的属性可以通过将 Tag 对象视为字典来访问。

```
atag = soup_atag.a
print (atag['href'] )

#output
http://www.packtpub.com
```

之前的代码通过访问 href 属性的值，打印出与第一个 `<a>` 标签关联的 URL（`http://www.packtpub.com`）。

可以使用 `.attrs` 属性访问与标签关联的不同属性。

`print(atag.attrs)` 代码行给出 `{'href':'http://www.packtpub.com'}`。

除了名称和属性之外，Tag 对象还有用于在文档中导航和搜索的辅助方法，我们将在以下章节中讨论。

### NavigableString 对象

NavigableString 对象保存 HTML 或 XML 标签内的文本。这是一个具有搜索和导航方法的 Python Unicode 字符串。有时我们可能需要根据当前文本导航到 HTML/XML 文档中的其他标签或文本。使用普通的 Python Unicode 字符串，搜索和导航方法将无法工作。NavigableString 对象将以 Unicode 字符串的形式为我们提供标签内的文本，同时提供用于搜索和导航树的各种方法。

我们可以使用 `.string` 获取存储在特定标签内的文本。

```
first_a_string = soup_atag.string
```

在之前的代码中，创建了 NavigableString 对象（`first_a_string`），它保存了第一个 `<a>` 标签内的字符串，u'Home'。

### 快速参考

您可以查看以下参考资料，以了解创建以下对象的概览：

- BeautifulSoup
  - `soup = BeautifulSoup(string)`
  - `soup = BeautifulSoup(string,features="xml")`  #for xml
- Tag
  - `tag = soup.tag`  #accessing a tag
  - `tag.name`        #Tag name
  - `tag['attribute']`  #Tag attribute

