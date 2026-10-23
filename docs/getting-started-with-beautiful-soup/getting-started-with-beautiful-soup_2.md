## 2 创建 BeautifulSoup 对象

我们在*第 1 章，安装 Beautiful Soup*中已经了解了如何在 Linux、Windows 和 Mac OS X 机器上安装 Beautiful Soup。

Beautiful Soup 广泛用于从网页中获取数据。我们可以使用 Beautiful Soup 提取 HTML/XML 文档中的任何数据，例如获取页面中的所有链接或页面内标签中的文本。为了实现这一目标，Beautiful Soup 为我们提供了不同的对象，以及简单的搜索和导航方法。

任何输入的 HTML/XML 文档都会被转换为不同的 Beautiful Soup 对象，基于这些对象的不同属性和方法，我们可以提取所需的数据。Beautiful Soup 中的对象列表包括：

- `BeautifulSoup`
- `Tag`
- `NavigableString`

### 创建 BeautifulSoup 对象

创建 BeautifulSoup 对象是任何 Beautiful Soup 项目的起点。BeautifulSoup 对象表示用于创建它的输入 HTML/XML 文档。

BeautifulSoup 通过传递一个字符串或一个类文件对象来创建（这个对象可以是我们机器上本地存储的文件的打开句柄，也可以是一个网页）。

### 从字符串创建 BeautifulSoup 对象

可以将字符串传递给 `BeautifulSoup` 构造函数来创建对象，如下所示：

```
helloworld = "<p>Hello World</p>"
soup_string = BeautifulSoup(helloworld)
```

![](img/c437561ffd11fadb571896e0a4ca4d18_26_0.png)

> **下载示例代码**
> 
> 你可以从自己的账户中下载所有已购买的 Packt 图书的示例代码文件，网址为 `http://www.packtpub.com`。如果你从其他渠道购买了本书，可以访问 `http://www.packtpub.com/support` 并注册，文件将会直接通过电子邮件发送给你。

上述代码将根据输入字符串 `helloworld` 创建 BeautifulSoup 对象。我们可以看到输入已被当作 HTML 处理，并可通过 `print(soup_string)` 查看该对象的内容。

```
<html><body><p>Helloworld</p></body></html>
```

![](img/c437561ffd11fadb571896e0a4ca4d18_26_1.png)

> 根据所使用的解析器，上述代码的输出在某些系统中可能会有所不同。这将在本章后面进行解释。

在创建对象的过程中，Beautiful Soup 会使用受支持的解析器将输入标记（HTML/XML）转换为树结构。这样，标记会被表示为不同的 Beautiful Soup 对象，例如 `BeautifulSoup`、`Tag` 和 `NavigableString`。

### 从类文件对象创建 BeautifulSoup 对象

也可以将类文件对象传递给 BeautifulSoup 构造函数来创建对象。这在解析在线网页时非常有用，这也是 Beautiful Soup 最常见的用途。

例如，假设我们需要获取由 Packt Publishing 出版的所有图书的列表，该列表位于 `http://www.packtpub.com/books`。为了减少通过浏览器访问此 URL 来获取页面内容为字符串的开销，更合适的做法是提供该 URL 的类文件对象来创建 BeautifulSoup 对象。

```
import urllib2
from bs4 import BeautifulSoup
```

```
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page)
```

在上述 Python 脚本中，我们使用了 urllib2 模块（Python 原生模块）来打开 http://www.packtpub.com/books 页面。`urllib2.urlopen()` 方法会为输入的 URL 返回一个类文件对象。然后我们通过传递该类文件对象来创建 BeautifulSoup 对象 `soup_packtpage`。

> 使用 URL 类文件对象创建 BeautifulSoup 对象是处理在线网页的高效方式。

在上一示例中，我们学习了如何通过为 URL 传递类文件对象来创建 BeautifulSoup 对象。类似地，我们也可以将本地文件的文件对象传递给 BeautifulSoup 构造函数。

为此，请从终端执行命令 mkdir Soup，在机器上创建一个本地文件夹。使用 touch Soup/foo.html 在该文件夹中创建一个 HTML 文件 foo.html。在同一终端中，使用 cd Soup 切换到刚创建的目录。

现在让我们来看一下使用 foo.html 文件创建 BeautifulSoup 的过程。

```
with open("foo.html","r") as foo_file:
    soup_foo = BeautifulSoup(foo_file)
```

上面几行代码会根据本地文件 foo.html 的内容创建一个 BeautifulSoup 对象。

BeautifulSoup 有一个基本的警告机制，用于提示我们传入的是文件名而不是文件对象。

让我们看一下下一行代码：

```
soup_foo = BeautifulSoup("foo.html")
```

这将产生以下警告：

UserWarning: "foo.html" looks like a filename, not markup. You should probably open this file and pass the filehandle into Beautiful Soup.

但仍然会创建一个 BeautifulSoup 对象，并假定我们传入的字符串（"foo.html"）是 HTML。

`print(soup_foo)` 代码行将给出以下输出：

```html
<html><body><p>foo.html</p></body></html>
```

同样的警告机制也会在我们尝试传入 URL 而不是 URL 文件对象时提醒我们。

```python
soup_url = BeautifulSoup("http://www.packtpub.com/books")
```

上一行代码将产生以下警告：

```text
UserWarning: "http://www.packtpub.com/books" looks like a URL.
Beautiful Soup is not an HTTP client. You should probably use
an HTTP client to get the document behind the URL, and feed
that document to Beautiful Soup
```

这里同样会创建 BeautifulSoup 对象，并将该字符串（URL）视为 HTML。

```python
print(soup_url)
```

上述代码将给出以下输出：

```html
<html><body><p>http://www.packtpub.com/books</p></body></html>
```

因此，我们应该向 BeautifulSoup 构造函数传入文件句柄或字符串。

### 创建用于 XML 解析的 BeautifulSoup 对象

Beautiful Soup 也可以用于 XML 解析。在创建 BeautifulSoup 对象时，Beautiful Soup 会选择 TreeBuilder 类来创建 HTML/XML 树。TreeBuilder 类用于根据输入文档创建 HTML/XML 树。默认行为是选择任意一个使用默认 HTML 解析器的 HTML TreeBuilder 对象，从而创建 HTML 树。在前面的示例中，使用字符串 helloworld 时，我们可以验证 soup_string 对象的内容，这表明输入在默认情况下被视为 HTML。

```python
soup_string = BeautifulSoup(helloworld)
print(soup_string)
```

上述代码片段的输出如下：

```html
<html><body><p>Hello World</p></body></html>
```

如果我们希望 Beautiful Soup 转而将输入解析为 XML，则需要使用 `BeautifulSoup` 构造函数中的 `features` 参数来明确指定这一点。通过指定 `features` 参数，BeautifulSoup 将能够选择满足我们所请求特性的最佳 `TreeBuilder`。

### 理解 features 参数

`TreeBuilders` 类使用底层解析器进行输入处理。每个 `TreeBuilder` 会根据其使用的解析器拥有不同的特性集。因此，输入会根据传递给构造函数的 `features` 参数而以不同的方式处理。Beautiful Soup 中目前由不同 `TreeBuilder` 使用的解析器如下：

- `lxml`
- `html5lib`
- `html.parser`

`BeautifulSoup` 构造函数的 `features` 参数可以接受字符串列表或字符串值。每个 `TreeBuilder` 及其底层解析器当前支持的特性如下表所示：

| 特性 | TreeBuilder | 解析器 |
|---|---|---|
| ['lxml','html','fast','permissive'] | LXMLTreeBuilder | lxml |
| ['html','html5lib','permissive','strict','html5'] | HTML5TreeBuilder | html5lib |
| ['html','strict','html.parser'] | HTMLParserTreeBuilder | html.parser |
| ['xml','lxml','permissive','fast'] | LXMLTreeBuilderForXML | lxml |

`features` 参数可以指定为字符串列表或字符串值。Beautiful Soup 会选择具有指定特性的最合适的 `TreeBuilder`。对于 HTML 文档，选择 TreeBuilder 的顺序取决于其构建所基于的解析器的优先级：首先是 `lxml`，其次是 `html5lib`，最后是 `html.parser`。例如，如果我们提供 `html` 作为特性，Beautiful Soup 会在 `lxml` 解析器可用时选择 `lxmlTreeBuilder`。如果 `lxml` 解析器不可用，它会选择基于 `html5lib` 解析器的 `HTML5TreeBuilder`；如果 `html5lib` 解析器也不可用，则选择基于 `html.parser` 的 `HTMLPraserTreeBuilder`。对于 XML，由于 `lxml` 是唯一可用的解析器，所以始终选择 `LXMLTreeBuilderForXML`。




### 创建 BeautifulSoup 对象

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




- NavigableString
  - `soup.tag.string` # 获取标签的字符串

### 小结

在本章中，我们学习了 Beautiful Soup 模块中的不同对象。我们理解了 HTML/XML 文档在底层 TreeBuilders 的帮助下如何被转换为 BeautifulSoup 对象。我们还研究了通过传递字符串和文件对象（用于本地文件和 URL）来创建 BeautifulSoup。同时解释了为 XML 解析创建 BeautifulSoup 以及在构造函数中使用 `features` 参数。我们看到了 HTML/XML 文档中的不同标签和文本如何被表示为 Beautiful Soup 中的 Tag 和 NavigableString 对象。

在下一章中，我们将学习 Beautiful Soup 提供的不同搜索方法，例如 `find()`、`find_all()` 和 `find_next()`。借助这些搜索方法，我们将能够从 HTML/XML 文档中提取数据，这确实是 Beautiful Soup 最强大的功能。
