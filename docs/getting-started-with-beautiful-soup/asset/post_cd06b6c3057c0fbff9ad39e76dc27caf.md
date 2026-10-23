
There are no longer gibberish characters because we have specified the correct encoding and we can verify this from the output.

The encoding that we pass should be correct, otherwise the character encoding will be wrong; for example, if we pass the encoding as latin-1 in the preceding HTML fragment, the result will be different.

```
soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")
print(soup.prettify())

#output
'The Spanish language is written using the Spanish alphabet, which
  is the Latin alphabet with one additional letter, eñe (ñ), for
a total of 27 letters.
```

So it is better to pass the encoding only if we are sure about the encoding used in the document.

## Output encoding

Encoding support is also present for the output text from Beautiful Soup. There are certain output methods in Beautiful Soup, for example, prettify(), which will give the output only in the UTF-8 encoding. Even though the encoding was something different like ISO 8859-2, the output will be in UTF-8. For example, the following HTML content is an example of ISO8859-2 encoding:

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

The soup.original_encoding will give us the encoding as ISO8859-2, which is true for the preceding HTML code snippet. But print (soup.prettify()) will give the output in utf-8.

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

Note that the meta tag got changed to `utf-8` to reflect the changes; also, the characters are different from the original content.

This is the default behavior of the `prettify()` method. But if we don't want the encoding to be changed to UTF-8, we can specify the output encoding by passing it in the `prettify()` method as shown in the following code snippet:

```
```python
print(soup.prettify("ISO8859-2")
```
```

The preceding code line will give the output in ISO8859-2 itself.

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

We can also call `encode()` on any Beautiful Soup object and represent it in the encoding we pass. The `encode()` method also considers UTF-8 encoding by default. This is shown in the following code snippet:

```
```python
print(soup.p.encode())

#output
cÃ©dille (from French), is a hook or tail ( Ł ) added under
certain letters as a diacritical mark to modify their
pronunciation
```
```

Like prettify(), encode() also takes a different encoding as its parameter.

```
print(soup.encode("ISO-8859-2")

#output
cédille (from French), is a hook or tail ( ç ) added under certain
letters as a diacritical mark to modify their pronunciation
```

## Quick reference

You can take a look at the following references to get an overview of the code present in this chapter:

- `soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")`. Here, from_encoding is used while creating BeautifulSoup to specify the document encoding.
- `soup.original_encoding`: This gives the original encoding detected by Beautiful Soup.
- The output content in specific encoding is listed using the following methods:
  - `soup.prettify()`
  - `soup.encode()`

## Summary

In this chapter, we saw the encoding support in Beautiful Soup. We understood how to get the original encoding detected by Beautiful Soup. We also learned to create a BeautifulSoup object by explicitly specifying the encoding. The output encoding was also discussed in this chapter. The next chapter deals with the different methods provided by Beautiful Soup to display content.

# 7
## Output in Beautiful Soup

Beautiful Soup not only searches, navigates, and modifies the HTML/XML, but also the output content in a good format. Beautiful Soup can deal with different types of printing such as:

- Formatted printing
- Unformatted printing

Apart from these, Beautiful Soup provides different formatters to format the output. Since the HTML tree can undergo modification after creation, these output methods will help in viewing the modified HTML tree.

Also in this chapter, we will discuss a simple method of getting only the text stored within a web page.

## Formatted printing

Beautiful Soup has two supported ways of printing. The first one is formatted printing that prints the current Beautiful Soup object into the formatted Unicode strings. Each tag is printed in a separate line with good indentation and this leads to the right look and feel. Beautiful Soup has the built-in method prettify() for formatted printing. For example:

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

The following screenshot shows the output:

![](img/c437561ffd11fadb571896e0a4ca4d18_104_0.png)

In the output, we can see that <html><body> gets appended. This is because Beautiful Soup uses the lxml parser and it identifies any string passed by default as HTML and performs the printing after appending the extra tags.

The prettify() method can be called either on a Beautiful Soup object or any of the tag objects. For example:

```
producer_entry = soup.ul
print(producer_entry.prettify())
```

## Unformatted printing

Beautiful Soup supports the plain printing of the BeautifulSoup and Tag objects. This will return only the plain string without any formatting.

This can be done by using the str() or the unicode() method.

## Output in Beautiful Soup

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

This HTML code will look as shown in the following screenshot:

![](img/c437561ffd11fadb571896e0a4ca4d18_106_0.png)

The left-most column represents the symbols and the corresponding HTML entities are represented in the next column. For the symbol &, the corresponding HTML entity code is &amp; likewise for the symbol ©, the code is &copy;.

The output methods in Beautiful Soup escape only the HTML entities of >, <, and & as &gt;, &lt;, and &amp;. Rest of the special entities are converted to Unicode while constructing the BeautifulSoup object, and upon output using prettify() or other methods, we get only the UTF-8 string of the HTML entities. We won't get the HTML entities back (except for &, >, and <).

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

Here we have created the soup object based on the text content for the HTML page in a browser that had the & symbol instead of the &amp; code. Likewise for other entities in the prettify() method, the output is shown as follows:

![](img/c437561ffd11fadb571896e0a4ca4d18_107_0.png)

We can understand that other HTML entities were converted to Unicode representation. Beautiful Soup allows output formatters to have a control over this behavior. There are the following four types of formatters available:

- minimal
- html
- None
- function
