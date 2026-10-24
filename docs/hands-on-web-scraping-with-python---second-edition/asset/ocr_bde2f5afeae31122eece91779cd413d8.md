# Applying condition for element 'calories' if text is less than 800

```
```python
for element in tree.iter():
    if element.tag=="calories":
        if int(element.text)>800:
            print(f"{element.text}")
```
```

# Applying condition for element 'calories' if text is less than 800 using XPath

```
```python
for element in tree.xpath('//food'):
    if int(element.xpath('./calories/text()')[0])<800:
        print(f"{element.xpath('./name/text()')[0]} - {element.xpath('./calories/text()')[0]}")
```
```

Belgian Waffles - 650
French Toast - 600

Figure 3.13: Iterating and applying logic with specific nodes and using XPath

As seen in this section, we need to parse the content first and then we can apply element-based logic to parts of content or whole content. Only basic usage of etree and its core features related to reading and traversing are used in this example. We will read and further process HTML documents in the next section.

# Reading HTML documents

In this example, we will pass a request to the URL `http://httpbin.org/forms/post` using `urllib.request`, parse the web content using `lxml.html`, and traverse through the elements as `lxml.html.HTMLElement`, applying XPath and CSS selectors. Let’s import and define the URL to be used, as seen in the following code:

```
```python
from lxml import html
from urllib.request import urlopen   #loading URL
url='http://httpbin.org/forms/post'
```
```

Now, we will pass a request to the defined url string, using `urlopen()`, and parse the response received. As seen in the following code, `parse()` returns `lxml.etree`, but using `getroot()`, we are receiving HTMLElement as root. As this example is regarding HTML, we are interested in HTMLElement:

```
```python
tree = html.parse(urlopen(url))  # load URL using urlopen and parse
type(tree)                     # lxml.etree._ElementTree
root = tree.getroot()          # returns the document root node <html>
type(root)                     # lxml.html.HtmlElement
```
```