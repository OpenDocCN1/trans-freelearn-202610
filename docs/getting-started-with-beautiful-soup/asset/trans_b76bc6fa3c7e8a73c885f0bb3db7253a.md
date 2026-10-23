

不再有乱码字符，因为我们已经指定了正确的编码，并且可以从输出中验证这一点。

我们传递的编码应该是正确的，否则字符编码会错误；例如，如果我们在前面的 HTML 片段中将编码传递为 latin-1，结果会不同。

```
soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")
print(soup.prettify())

#output
'The Spanish language is written using the Spanish alphabet, which
  is the Latin alphabet with one additional letter, eñe (ñ), for
a total of 27 letters.
```

因此，最好只有在确定文档使用的编码时才传递编码。

## 输出编码

Beautiful Soup 对输出文本也提供编码支持。Beautiful Soup 中存在某些输出方法，例如 prettify()，它只会以 UTF-8 编码给出输出。即使编码是不同的东西，比如 ISO 8859-2，输出仍为 UTF-8。例如，以下 HTML 内容是 ISO8859-2 编码的示例：

```
html_markup = """
<html>
  <meta http-equiv="Content-Type"
    content="text/html;charset=ISO8859-2"/>
  <p>cédille (from French), is a hook or tail ( ž )  added under
    certain letters as a diacritical mark to modify their
    pronunciation
  </p>"""
soup = BeautifulSoup(html_markup,"lxml")
```

soup.original_encoding 将会给出 ISO8859-2 的编码，这对于前面的 HTML 代码片段是正确的。但 print (soup.prettify()) 将会以 utf-8 给出输出。

```
<html>
  <head>
    <meta content="text/html;charset=utf-8" http-equiv="Content-Type"/>
```

```
```html
</head>
<body>
  <p>
    cÃ©dille (from French), is a hook or tail ( Ł ) added under
    certain letters as a diacritical mark to modify their
    pronunciation
  </p>
</body>
</html>
```
```

请注意 meta 标签已更改为 `utf-8` 以反映更改；此外，字符与原始内容不同。

这是 `prettify()` 方法的默认行为。但如果我们不想让编码更改为 UTF-8，我们可以通过在 `prettify()` 方法中传递它来指定输出编码，如以下代码片段所示：

```
```python
print(soup.prettify("ISO8859-2")
```
```

前面的代码行将以 ISO8859-2 本身给出输出。

```
```html
<html>
  <head>
    <meta content="text/html;charset=ISO-8859-2" http-equiv="Content-Type"/>
  </head>
  <body>
    <p>
      cédille (from French), is a hook or tail ( ž ) added under
      certain letters as a diacritical mark to modify their
      pronunciation
    </p>
  </body>
</html>
```
```

我们也可以在任何 Beautiful Soup 对象上调用 `encode()` 并将其表示为我们传递的编码。`encode()` 方法默认也考虑 UTF-8 编码。这在以下代码片段中显示：

```
```python
print(soup.p.encode())

#output
cÃ©dille (from French), is a hook or tail ( Ł ) added under
certain letters as a diacritical mark to modify their
pronunciation
```
```

与 prettify() 类似，encode() 也接受不同的编码作为其参数。

```
print(soup.encode("ISO-8859-2")

#output
cédille (from French), is a hook or tail ( ç ) added under certain
letters as a diacritical mark to modify their pronunciation
```

## 快速参考

您可以查看以下参考资料，以了解本章中代码的概览：

- `soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")`。这里，from_encoding 在创建 BeautifulSoup 时使用，以指定文档编码。
- `soup.original_encoding`：这会给出 Beautiful Soup 检测到的原始编码。
- 特定编码的输出内容使用以下方法列出：
  - `soup.prettify()`
  - `soup.encode()`

## 总结

在本章中，我们看到了 Beautiful Soup 中的编码支持。我们了解了如何获取 Beautiful Soup 检测到的原始编码。我们还学习了通过显式指定编码来创建 BeautifulSoup 对象。输出编码也在本章中进行了讨论。下一章将介绍 Beautiful Soup 提供的用于显示内容的各种方法。

# 7
## Beautiful Soup 中的输出

Beautiful Soup 不仅搜索、导航和修改 HTML/XML，还以良好的格式输出内容。Beautiful Soup 可以处理不同类型的打印，例如：

- 格式化打印
- 非格式化打印

除此之外，Beautiful Soup 还提供不同的格式化程序来格式化输出。由于 HTML 树在创建后可能会发生修改，这些输出方法有助于查看修改后的 HTML 树。

此外，本章中我们还将讨论一种从网页中获取仅存储的文本的简单方法。

## 格式化打印

Beautiful Soup 有两种支持的打印方式。第一种是格式化打印，它将当前的 Beautiful Soup 对象打印为格式化的 Unicode 字符串。每个标签都在单独的行中打印，并带有良好的缩进，这导致了正确的外观和感觉。Beautiful Soup 有内置的 prettify() 方法用于格式化打印。例如：

```
html_markup = """<p class="ecopyramid">
<ul id="producers">
    <li class="producerlist">
        <div class="name">plants</div>
        <div class="number">100000</div>
    </li>
    <li class="producerlist">
        <div class="name">algae</div>
```

```
<div class="number">100000</div>
</li>
</ul>"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.prettify())
```

以下截图显示了输出：

![](img/c437561ffd11fadb571896e0a4ca4d18_104_0.png)

在输出中，我们可以看到 <html><body> 被附加了。这是因为 Beautiful Soup 使用 lxml 解析器，并且它默认将传递的任何字符串识别为 HTML，并在附加额外标签后执行打印。

prettify() 方法可以调用在 Beautiful Soup 对象或任何标签对象上。例如：

```
producer_entry = soup.ul
print(producer_entry.prettify())
```

## 非格式化打印

Beautiful Soup 支持 BeautifulSoup 和 Tag 对象的纯文本打印。这将仅返回纯字符串而没有任何格式。

这可以通过使用 str() 或 unicode() 方法来完成。

## Beautiful Soup 中的输出

```
<th>&amp;</th>
    <td align="center">&amp;amp;</td>
    <td align="center">ampersand</td>
</tr>
<tr>
    <th>&cent;</th>
    <td align="center">&amp;cent;</td>
    <td align="center">cent</td>
</tr>
<tr>
    <th>&copy;</th>
    <td align="center">&amp;copy;</td>
    <td align="center">copyright</td>
</tr>
<tr>
    <th>&#247;</th>
    <td align="center">&amp;divide;</td>
    <td align="center">divide</td>
</tr>
<tr>
    <th>&gt;</th>
    <td align="center">&amp;gt;</td>
    <td align="center">greater than</td>
</tr>
</table>
</body>
</html>
```

此 HTML 代码将如下所示的截图中所示：

![](img/c437561ffd11fadb571896e0a4ca4d18_106_0.png)

最左边的列代表符号，对应的 HTML 实体在下一列中表示。对于符号 &，对应的 HTML 实体代码是 &amp; 同样对于符号 ©，代码是 &copy;。

Beautiful Soup 中的输出方法仅转义 >, < 和 & 的 HTML 实体，分别转义为 &gt;、&lt; 和 &amp;。其余特殊实体在构建 BeautifulSoup 对象时被转换为 Unicode，并在使用 prettify() 或其他方法输出时，我们只得到 HTML 实体的 UTF-8 字符串。我们不会得到 HTML 实体返回（除了 &、> 和 <）。

```
html_markup = """<html>
<body>& &amp; ampersand
¢ &cent; cent
© &copy; copyright
÷ &divide; divide
> &gt; greater than
</body>
</html>
"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.prettify())
```

这里我们基于浏览器中 HTML 页面文本内容创建了 soup 对象，该内容在浏览器中具有 & 符号而不是 &amp; 代码。同样，对于 prettify() 方法中的其他实体，输出如下所示：

![](img/c437561ffd11fadb571896e0a4ca4d18_107_0.png)

我们可以理解其他 HTML 实体被转换为 Unicode 表示。Beautiful Soup 允许输出格式化程序控制此行为。以下是可用的四种格式化程序类型：

- minimal
- html
- None
- function

