Normally, two types of parsers are supported by BeautifulSoup:

+   - Type of markup: html, xml, and html5
+   - Name of parser library: lxml, html5lib, and html.parser (lxml and html5lib must be installed to be used; html.parser is a built-in HTML parser)

> **Important note**
> lxml is recommended as the best parser to be used with BeautifulSoup, because of its memory and speed. html5lib is usually the second preference, but the parsed output can also be used to determine the preference of the parser to be used based on the content or DOM requirements.

It’s also to be noted that the parsed tree (output) or parsed content generated will differ based on the parser used. To demonstrate parsing, here, a markup variable with an incomplete error code has been declared:

```
markup = "<a></p></a></p>"
```

The following code uses the default parser for markup:

```
BSOup(markup)    # <html><body><a></a></body></html>
```

The html.parser parser has been used and only <a> has been parsed; the </p> HTML tag has not been returned from the markup string variable:

```
BSOup(markup,"html.parser")      # <a></a>
```

The lxml parser and the default parser outputs look similar:

```
BSOup(markup,"lxml")    # <html><body><a></a></body></html>
```

The html5lib parser seems to provide more detailed information in comparison to other parsers, such as lxml and html.parser:

```
BSOup(markup,"html5lib")
# <html><head></head><body><a><p></p></a><p></p></body>
  </html>
```

All incomplete tags have been listed in the output, and the complete HTML structure (<html>, <head>, <body>) has also been maintained.