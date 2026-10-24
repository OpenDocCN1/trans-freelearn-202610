The Python libraries that are required for this chapter are as follows:

+   - pyquery
+   - urllib
+   - requests

The code files for this chapter are available online on GitHub: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/tree/main/Chapter04

# PyQuery overview

PyQuery is a jQuery-like library for Python that facilitates the easy implementation and use of lxml and CSS selectors.

As the name suggests, PyQuery enhances query-related procedures (XPath and CSS selector expressions) with short and readable lines of code. Web scraping, as you all are aware, requires parsing and traversing features that reside on top of various types of web documents.

PyQuery provides additional features related to DOM and ElementTree, and uses CSS selectors to perform queries. The purpose of using PyQuery expressions (or queries) is similar to that of XPath or CSS selector-based expressions. PyQuery is almost the same as jQuery for web documents.

The following list contains a basic comparison of expressions that collect the href attribute from the <a class="main"> element:

+   - PyQuery: response.find('a.main').attr('href')
+   - lxml XPath: response.xpath(".//a[contains(@class,'main')]/@href")
+   - lxml cssselect: response.cssselect("a.main").get('href')
+   - jQuery: $('a.main').attr('href');

> **Important note**
> For more details on PyQuery and its latest documentation, please visit https://pyquery.readthedocs.io/en/latest/ and https://pypi.org/project/pyquery/. Similarly, for lxml (XPath and cssselect), visit https://lxml.de/index.html.

After our short introduction to PyQuery and its similarities with XPath and CSS selectors, let's go over the basics of jQuery.