## 6 Beautiful Soup 中的编码支持

所有网页都会有一个关联的编码。现代网站有不同的编码，如 UTF-8 和 Latin-1。如今，UTF-8 是网站使用的编码标准。因此，在抓取此类页面时，抓取器也必须能够理解这些编码。否则，用户会在网页浏览器中看到某些字符，而使用抓取器获得的结果将是乱码字符。例如，考虑来自 Wikipedia 的一个示例网页内容，我们可以看到西班牙语字符 ñ。

### 字母表

西班牙语使用西班牙字母书写，它是拉丁字母加上一个额外的字母 ñ，共计 27 个字母。

| Letter | A | B | C | D | E | F | G | H | I | J | K | L | M | N | Ñ | O | P | Q | R | S | T | U | V | W | X | Y | Z |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Name | a | be, be larga, be alta | ce | de | e | efe | ge | hache | i | jota | ka | ele | eme | ene | eñe | o | pe | cu | erre | ese | te | u | ve, uve, ve corta, ve baja | uve doble, doble ve, doble uve | equis | i griega, ye | zeta |
| IPA | /a/ | /b/ | /k/, /θ/ | /d/ | /e/ | /f/ | /g/, /x/ | silent | /i/ | /x/ | /k/ | /l/ | /m/ | /n/ | /ɲ/ | /o/ | /p/ | /k/ | /r/, /ɾ/ | /s/ | /t/ | /u/ | /b/ | /ɡw/, /β/ | /ks/, /ɣ/, /s/ | /ʝ/, /ʎ/ | /θ/ |

如果我们将相同的内容通过不支持网站之前使用的编码的抓取器运行，我们可能会得到以下内容：

```
The Spanish language is written using the Spanish alphabet, which
is the Latin alphabet with one additional letter,
e&#21336;e (&#21336;), for a total of 27 letters.
```

我们看到西班牙语字符 ñ 被乱码字符替换。因此，抓取器必须支持不同的编码。Beautiful Soup 处理这些编码非常简洁。在本章中，我们将看到 Beautiful Soup 中的编码支持。

### Beautiful Soup 中的编码

如前所述，每个 HTML/XML 文档都将以特定的字符集编码编写，例如 UTF-8 和 Latin-1。在 HTML 页面中，这使用 meta 标签表示，如以下示例所示：

```
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
```

Beautiful Soup 使用 UnicodeDammit 库自动检测文档的编码。Beautiful Soup 在从文档创建 soup 对象时将内容转换为 Unicode。

> Unicode 是一个字符集，即具有唯一数字的字符列表。例如，在 Unicode 字符集中，字符 B 的数字为 42。UTF-8 编码是一种用于将这些数字转换为二进制表示的算法。

在前面的示例中，Beautiful Soup 将文档转换为 Unicode。

```
html_markup = """<p> The Spanish language is written using the
Spanish alphabet, which is the Latin alphabet with one
additional letter, eñe (ñ), for a total of 27 letters.</p>
"""

soup = BeautifulSoup(html_markup,"lxml")
print(soup.p.string)

#output
The Spanish language is written using the Spanish alphabet, which
is the Latin alphabet with one additional letter, eñe (ñ), for
a total of 27 letters.
```

从之前的输出可以看出，额外字母部分（e単e (単)）存在差异，因为实际表示的地方出现了乱码字符。这是由于 UnicodeDammit 对文档原始编码的错误解释造成的。

### 了解 HTML 文档的原始编码

之前的 HTML 内容原本是 UTF-8 编码。但 Beautiful Soup 在生成 soup 对象时使用 UnicodeDammit 库检测编码，我们可以通过 `original_encoding` 属性获取此原始编码。

`soup.original_encoding` 将给出原始编码，在前一种情况下为 `euc-jp`。这是错误的，因为 UnicodeDammit 将编码检测为 `euc-jp` 而非 `utf-8`。

### 指定 HTML 文档的编码

UnicodeDammit 库在仔细搜索整个文档后检测编码。这耗时且在某些情况下 UnicodeDammit 库会错误地检测编码，如前所述。这种错误猜测大多发生在内容非常短且编码之间存在相似性的情况下。如果我们事先知道 HTML 文档的编码，我们可以避免这种情况。我们可以将编码传递给 BeautifulSoup 构造函数，从而避免不必要的耗时和错误猜测。编码作为 `from_encoding` 参数指定在构造函数的一部分中。因此，在前一种情况下，我们可以将编码指定为 `utf-8`。

```
soup = BeautifulSoup(html_markup,"lxml",from_encoding="utf-8")
print(soup.prettify())
```

```
#output
<html>
  <body>
    <p>
      The Spanish language is written using the Spanish alphabet,
      which is the Latin alphabet with one additional letter,
      eñe (ñ), for a total of 27 letters.
    </p>
  </body>
</html>
```



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

### 输出编码

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

### 快速参考

您可以查看以下参考资料，以了解本章中代码的概览：

- `soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")`。这里，from_encoding 在创建 BeautifulSoup 时使用，以指定文档编码。
- `soup.original_encoding`：这会给出 Beautiful Soup 检测到的原始编码。
- 特定编码的输出内容使用以下方法列出：
  - `soup.prettify()`
  - `soup.encode()`

### 总结

在本章中，我们看到了 Beautiful Soup 中的编码支持。我们了解了如何获取 Beautiful Soup 检测到的原始编码。我们还学习了通过显式指定编码来创建 BeautifulSoup 对象。输出编码也在本章中进行了讨论。下一章将介绍 Beautiful Soup 提供的用于显示内容的各种方法。
