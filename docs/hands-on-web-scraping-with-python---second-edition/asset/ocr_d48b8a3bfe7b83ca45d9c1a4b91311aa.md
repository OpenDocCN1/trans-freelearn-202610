Let's verify the installation and version of bs4 (the complete code is available on GitHub: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter05/bs4_setup.ipynb):

```
import bs4
bs4.__version__        # 4.11.1
dir(bs4)
# ['BeautifulSoup', 'BeautifulStoneSoup', 'CData', 'Comment',
'Counter', 'DEFAULT_OUTPUT_ENCODING', 'Declaration', 'Doctype',...,
'SoupStrainer', 'StopParsing', 'Stylesheet', 'Tag',..., '__version__',
'_s', '__soup', 'builder', 'builder_registry', 'dammit', 'element',
'formatter', 'os', 're', 'sys', 'traceback', 'warnings']
```

The preceding code imports bs4 (Beautiful Soup), prints its version information, which is 4.11.1, and explores bs4. We can see in the output that there are a few important classes available, such as BeautifulSoup and SoupStrainer.

In this section, we have installed Beautiful Soup and verified its installation along with its version information. In the next section, we will explore various features of Beautiful Soup.

# Exploring Beautiful Soup

We will now explore a few select, important features and classes in Beautiful Soup, also known as the bs4 Python library. Activities such as parsing web documents and using Beautiful Soup for traversing, searching, and iterating will be covered in this section.

In carrying out parsing and using the features of bs4, we will be using the BeautifulSoup and SoupStrainer classes to target certain sections or elements of web documents:

```
from bs4 import BeautifulSoup as BSoup
from bs4 import SoupStrainer
```

As seen in the preceding code, the BeautifulSoup and SoupStrainer classes are imported from bs4. In the next section, we will demonstrate parsing using bs4.

# Parsing

As discussed in previous sections, parsing is an activity that rectifies errors in content. Using a parser is always recommended to obtain similar results from web content across platforms and systems. Also, we need to plan for or be ready to accept that the project will take longer than usual due to parsing when using BeautifulSoup.

HTML content or code blocks will be passed to BeautifulSoup, also known as the BSoup constructor, along with the selected parser or a parser similar to BSoup (that can be a string or HTML parser). If no parser is specified, the system will use the default HTML parser, which is usually lxml.