

来自所有可用的 `<loc>...</loc>` 标记的数据；同样地，`mods` 列出所有 `<lastmod>...</lastmod>` 标记中的内容：

```python
locs = re.findall(r"<loc>(.*)</loc>", source)
mods = re.findall(r"<lastmod>(.*)</lastmod>", source)
```

`locFinals` 是一个 Python 列表，其中包含来自 `locs` 和 `mods` 的各个元素，并以元组形式呈现。Python 的 `zip(locs, mods)` 函数的作用类似于合并器，它合并传入的参数，并将迭代得到的 `locs` 和 `mods` 值创建为一个元组：

```python
locFinals = list(zip(locs, mods))
print(locFinals[0])    # (locs[0], mods[0])
('https://www.zyte.com/blog/json-parsing-with-python/', '2023-04-06T14:42:05+00:00')
```

`locFinals` 包含多个元素。在迭代时，我们同时进行元组解包，并使用 `re.sub()` 和 `re.split()` 对数据进行清理和拆分，以便获取目标数据：

```python
for locFinal in locFinals:
    loc, datetime = locFinal
    # unpacking tuple (extracting tuple elements)
    loc = re.sub('^(http\:|https\:)', '', loc)
    # cleaning
    loc = re.sub('[\/]{2}', '', loc)
    loc = re.sub('\/$', '', loc)
    locSplit = re.split(r"/", loc)
    # breaking
    category = locSplit[1]
    categoryTopic = re.sub('[-]', ' ', locSplit[2])
    ymdTime = re.split("T", datetime)
    # separating date and time
    ymd = ymdTime[0]
    time = re.sub("\+.*", "", ymdTime[1])
    # cleaning
```

最终，识别出的目标数据被加载到 `dataSet` 数据容器中：

```python
dataSet.append(["Zyte", category, categoryTopic, ymd, time])
print(dataSet)
[['Zyte', 'blog', 'json parsing with python', '2023-04-06', '14:42:05'], ...,
 ['Zyte', 'webinars', 'the right data fields for e commerce data project', '2023-03-21', '06:17:35'], ...]
```

如本示例所示，re 方法（`findall()`、`sub()` 和 `split()`）以及 Python 的 `zip()` 方法发挥了重要作用。通过结合 Python re 和内建方法，可以高效且轻松地完成基于正则表达式的数据提取。

### 示例 3——Godfrey 的经销商

在本示例中，我们将从 `https://godfreysfeed.com/dealersandlocations` 提取经销商详细信息。

使用 Python 的 requests 库发起 HTTP 请求，并将响应收集为 source：

```python
url = "https://godfreysfeed.com/dealersandlocations"
source = requests.get(url).text
```

我们将从 source 中获取与目标列（如 Name、Address、City、State、Zip、Lat 和 Lng）相关的数据。经销商相关数据仅在 *JavaScript* 中可用。这些数据还分散在多行中，并且包含一些 HTML 标签，如下所示：

```javascript
var infoWindowContent = "<div style='overflow:hidden; width:200px'>";
infoWindowContent = infoWindowContent + '<strong><span style='color:#e5011c;'>American Cowboy Shop</span></strong><br><strong>513 D Murphy Hwy</strong><br><strong>Blairsville, GA</strong><br><strong>30512</strong><br><br>';
infoWindowContent += '</div>';
var infowindow = new google.maps.InfoWindow({maxWidth: 450});
infowindow.setContent(infoWindowContent);
var latLng = new google.maps.LatLng(34.8752421, -83.9716038);
```

定义正则表达式以使用 `latlngs` 变量收集所有坐标数据，而 `contents` 则保存承载地址信息的 `infoWindowContent` 变量。最后，使用 Python 内建的 `zip()` 创建 `details`：

```python
latlngs = re.findall(r"var\s+latLng.*\.LatLng\((.*)\);", source)
contents = re.findall(r"infoWindowContent\+\s*\"(.*)\";", source)
details = list(zip(contents, latlngs))  # zip
```

现在，我们已在 `details` 中识别出 `latlngs` 和 `contents` 数据。接下来需要对 `details` 进行迭代，将 `detail` 元组解包为一组 `fullAddress` 和 `coordinate`：

```python
for detail in details:
    fullAddress, coordinate = detail
```

大家可以了解到，*Word 文档*、*PowerPoint 演示文稿*、*HTML*、*Jupyter 笔记本*、*来自各种应用程序的分析报告*，以及更多内容类型都支持将文件导出并保存为 PDF。

我们经常会在 PDF 文件中发现各种类型的数据（例如文本、表格和图像）。在*第 3 章和第 4 章*中，我们了解了如何使用 Python 提取基于 Web 的内容。在这里，我们将使用 PyPDF2 Python 库从 PDF 文件中提取数据。

在接下来的小节中，我们将从数据提取的角度安装并探索 PyPDF2。

## PyPDF2 库

PyPDF2（https://pypdf2.readthedocs.io/en/3.0.0/index.html）是一个免费、开源（https://github.com/py-pdf/PyPDF2）的 Python 库，用于处理 PDF 文件，并因其丰富的功能在 Python 开发者和社区中广受欢迎。

以下是它的一些主要功能：

- 文本提取
- 图像提取
- 元数据（文档详细信息）的可用性与提取
- 文件转换（PDF 转 Word 等）
- 文本修改（PDF 文件）
- 为现有或新建的 PDF 文件添加水印
- 添加安全性，例如为 PDF 文件设置密码保护
- 将 PDF 文件拆分为多个页面
- 将多个 PDF 文件合并为一个 PDF
- 处理页面布局、裁剪、变换和方向（例如旋转页面）

还应注意，PDF 文件可以使用 PDF 应用程序创建，也可以从其他文档格式导出并保存。虽然 PDF 文件保留了原始文档的适当格式，但其内容不像 HTML 或基于标记的文档那样。PDF 内容以原始格式（包含换行符、空格和制表符）存在，因此几乎必须使用正则表达式来定位和提取所需信息。

> **重要提示**
> 有大量处理 PDF 文件的 Python 库，例如 `pdfminer`、`pdfquery`、`xpdf`、`pdfrw`、`pikepdf` 和 `pymupdf`。在上述这些库中，PyPDF2 是我最喜欢的，因为它具有多种功能。正如 https://pypdf2.readthedocs.io/en/3.0.0/user/installation.html 中所介绍的，PyPDF2 可用于广泛的用例，也可以专注于某个特定领域，例如*加密（加密或解密）*和*图像处理*。

在下一节中，我们将使用 PyPDF2 来收集数据。

## 使用 PyPDF2 进行提取

PyPDF2 是一个完整的软件包，其中有各种类来处理其不同的功能。在提取方面，我们将使用 PyPDF2 中的 `PdfReader` 类（https://pypdf2.readthedocs.io/en/3.0.0/modules/PdfReader.html）。

如下代码所示，要进行提取，需要从 PyPDF2 导入 `PdfReader` 类。目标文件或文件路径（`pdffile`）被提供给 `PdfReader`。`reader` 是一个对象，承载着 `PdfReader` 的方法和属性：

```python
from PyPDF2 import PdfReader
pdffile = "python_cheat_sheet_v1.pdf" # file path or name
reader = PdfReader(pdffile)
# reader = PdfReader(open(pdffile,'rb'))
dir(reader)
['__class__',..., '__encryption__',...'isEncrypted', 'is_encrypted',
 'metadata', 'namedDestinations', 'named_destinations', 'numPages',
 'outline', 'outlines', 'pageLayout', 'pageMode', 'page_layout',
 'page_mode', 'pages', 'pdf_header', 'read', 'readNextEndLine',
 'readObjectHeader', 'read_next_end_line', 'read_object_header',
 'resolved_objects', 'stream', 'strict', 'threads', 'trailer',...]
```

PyPDF2 最近进行了一次更新，为其配备了新的方法和属性。此外，还有一些已弃用的实体，你在探索 PyPDF2 时可能会遇到。

PDF 的页数可以通过 `len(reader.pages)` 获取，而单个页面的来源或内容可以通过 `page1 = reader.pages[0]` 来定位（此处，索引 0 指第 1 页，1 指第 2 页，依此类推）。确定页码后，可以使用 `extract_text()` 方法提取内容：

```python
page1 = reader.pages[0]    # point to first page
page1_source = page1.extract_text()    # extract from first page
len(page1_source)                # 964
page1_source[:100]  # Python is a beautiful language. It's easy to learn and fun, and its syntax is simple yet elegant. Pyt
page1_source
"Python is a beautiful language. It's easy to learn and fun, and its syntax is simple yet elegant. ..... ...... Python Cheat Sheet\n1. Primitives Numbers"
```

上述代码使用 PyPDF2 加载了 PDF 文件的内容或页面。在接下来讨论的示例中，我们将应用正则表达式从 PDF 中提取所需内容。

