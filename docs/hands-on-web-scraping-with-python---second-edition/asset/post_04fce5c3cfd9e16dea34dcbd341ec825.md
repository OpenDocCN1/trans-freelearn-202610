
The Python libraries that are required for this chapter are as follows:

- pyquery
- urllib
- requests

The code files for this chapter are available online on GitHub: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter04

## PyQuery overview

PyQuery is a jQuery-like library for Python that facilitates the easy implementation and use of lxml and CSS selectors.

As the name suggests, PyQuery enhances query-related procedures (XPath and CSS selector expressions) with short and readable lines of code. Web scraping, as you all are aware, requires parsing and traversing features that reside on top of various types of web documents.

PyQuery provides additional features related to DOM and ElementTree, and uses CSS selectors to perform queries. The purpose of using PyQuery expressions (or queries) is similar to that of XPath or CSS selector-based expressions. PyQuery is almost the same as jQuery for web documents.

The following list contains a basic comparison of expressions that collect the href attribute from the <a class="main"> element:

- PyQuery: response.find('a.main').attr('href')
- lxml XPath: response.xpath(".//a[contains(@class,'main')]/@href")
- lxml cssselect: response.cssselect("a.main").get('href')
- jQuery: $('a.main').attr('href');

> **Important note**
> For more details on PyQuery and its latest documentation, please visit https://pyquery.readthedocs.io/en/latest/ and https://pypi.org/project/pyquery/. Similarly, for lxml (XPath and cssselect), visit https://lxml.de/index.html.

After our short introduction to PyQuery and its similarities with XPath and CSS selectors, let's go over the basics of jQuery.

## Introducing jQuery

jQuery (*write less, do more; see https://jquery.com/*) is one of the most popular JavaScript libraries and is lightweight, CSS3-compliant, cross-browser compatible, and quick. It supports plenty of features related to web documents, such as the DOM, HTML, and CSS.

Web-based document traversal, manipulation, event handling, animation, and AJAX are some of the core features jQuery is applied to. PyQuery expressions are similar to jQuery expressions, so anyone with prior jQuery and JavaScript knowledge will find it easy to use PyQuery with Python.

Throughout the chapter, you will be focused on using Python libraries only. Web development today involves JavaScript in relation to dynamism, development and testing, and many other aspects. For a full stack developer or web-related programmer, knowledge of JavaScript, along with AJAX and jQuery, is almost compulsory. Please visit https://learn.jquery.com/ for more information on jQuery.

You've received a basic introduction to PyQuery and jQuery in this section. The next section will explore PyQuery in depth and show you how to use it to write a crawling script or scrape a website.

## Exploring PyQuery

PyQuery addresses **DOM**-based expressions or queries in a quick, easy, and effective manner. In this section, we will install PyQuery and explore a few of its important features so that we are ready to develop a web scraping script.

> **Important note**
> You can find plenty of Python libraries that are similar to PyQuery. A few examples are parsel (https://pypi.org/project/parsel/), beautifulsoup (https://pypi.org/project/beautifulsoup4/), selectolax (https://pypi.org/project/selectolax/), and, of course, lxml (https://pypi.org/project/lxml/).

## Installing PyQuery

Please refer to the *Technical requirements* section before proceeding with installing the PyQuery library. With the help of the virtual environment (`secondEd`) you created in *Chapter 2*, install or update PyQuery using pip:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_107_0.png)

Figure 4.1: Installing PyQuery

As shown in Figure 4.1, installing PyQuery also installs or updates the lxml and cssselect libraries. To verify the installation, let’s import PyQuery using the following code:

```
import pyquery
print(dir(pyquery)) # Explore pyquery
# ['PyQuery', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', 'cssselectpatch', 'openers', 'pyquery', 'text']
```

In the preceding code, we imported PyQuery and checked the resources of the library using dir(). There are some functions and classes; here, we will be mostly using the PyQuery class:

```
print(dir(pyquery.PyQuery)) #Class PyQuery provides DOM related features
['Fn', '__add__',..., 'add_class', 'after', 'append', 'appendTo', 'append_to', 'attr', 'base_url', 'before', 'children', 'clear', 'clone', 'closest', 'contents', 'copy', 'count', 'css', 'each', 'empty', 'encoding', 'end', 'eq', 'extend', 'filter', 'find', 'fn', 'hasClass', 'has_class', 'height', 'hide', 'html', 'index', 'insert', 'insertAfter', 'insertBefore', 'insert_after', 'insert_before', 'is_', 'items', 'length', 'make_links_absolute',..., 'parent', 'parents', 'pop', 'prepend', 'prependTo', 'prepend_to', 'prev', 'prevAll', 'prev_all', 'remove', 'removeAttr', 'removeClass', 'remove_attr', 'remove_class', 'remove_namespaces', 'replaceAll', 'replaceWith', 'replace_all', 'replace_with', 'reverse', 'root', 'serialize', 'serializeArray', 'serializeDict', ..., 'show', 'siblings', 'size', 'sort', 'text', 'toggleClass', 'toggle_class', 'val', 'width', 'wrap', 'wrapAll', 'wrap_all', 'xhtml_to_html']
```

In the preceding code, we explored the PyQuery class and found many of the methods and attributes that somewhat resemble DOM concepts such as attr, children, eq, root, parent, next, and hasClass.

Important note
Code and outputs are copied from the Jupyter notebook. A single-line comment with # has been used to present short outputs and comments. If the output is spread across multiple lines, then a new line with # has been used. Also, multi-line outputs will be shortened as required.

In this section, you installed PyQuery and learned about a few of its features. In the next section, you will explore some examples of using PyQuery.

## Loading a web URL

We will now practically explore PyQuery’s features with the help of some code examples. Various types of web documents are available. We will be targeting HTML and XML content using the requests library as follows:

```
```python
from pyquery import PyQuery as pq
import requests
response = requests.get
    ("https://webscraper.io/test-sites/")
type(response)       # requests.models.Response
source = pq(response.content) # convert HTTP Response to pyquery
type(source) # pyquery.pyquery.PyQuery
```
```

As you can see in the preceding code, we have used the requests library for HTTP communication. In this example, we are processing the https://webscraper.io/test-sites/ URL, and we are checking the type of response and source. We need to convert the page source or HTTP response into a PyQuery object before processing the DOM using pyquery.source = pq(response.content) converts the response.content page source from the web into a PyQuery object.

With the PyQuery object, source, in hand, we are now ready to move on to the next section, where we will explore more features of PyQuery.

## Element traversing, attributes, and pseudo-classes

With the source PyQuery object from the previous section, we will explore some of the attributes and methods that PyQuery provides. You are also advised to go through the page source in the browser, or to identify or inspect elements using DevTools, before implementing the code.

These examples are handy for analyzing the elements and their contents in the page source:

```
```python
source.find('title')          # [<title>]
source.find('title').text()   # Web Scraper Test Sites

source.find('a')              # source('a')
# [<a>,..., <a.menuitem>, <a.menuitem>, <a.menuitem.dropdown-
toggle>, <a>,………, <a>, <a.btn-menu1.install-extension>, <a.btn-menu2>,
<a>, <a>, <a>,……, <a>, <a>, <a>]
```
```

The preceding code uses the find() method to look for the element, tag, or CSS selector provided to it and identifies those elements. find(a) returns the list of <a> elements along with a few class names, such as menuitem and btn-menu. After finding the elements, there are various methods, such as text(), attr(), and html(), that return the desired content or capture the key:value using attribute names or expressions. In the preceding case (source.find('title').text()), text() returns the contents found in <title> tags.
