# Using get_text()

Getting just text from websites is a common task. Beautiful Soup provides the method get_text() for this purpose.

If we want to get only the text of a Beautiful Soup or a Tag object, we can use the get_text() method. For example:

```
html_markup = """<p class="ecopyramid">
<ul id="producers">
    <li class="producerlist">
        <div class="name">plants</div>
        <div class="number">100000</div>
    </li>
    <li class="producerlist">
        <div class="name">algae</div>
        <div class="number">100000</div>
    </li>
</ul>"""
soup = BeautifulSoup(html_markup,"lxml")
print(soup.get_text())

#output
plants
100000

algae
100000
```

The get_text() method returns the text inside the Beautiful Soup or Tag object as a single Unicode string. But get_text() has issues when dealing with web pages. Web pages often have JavaScript code, and the get_text() method returns the JavaScript code as well. For example, in Chapter 3, Search Using Beautiful Soup, we saw the example of scraping book details from packtpub.com.

```
import urllib2
from bs4 import BeautifulSoup
url = "http://www.packtpub.com/books"
page = urllib2.urlopen(url)
soup_packtpage = BeautifulSoup(page,"lxml")
```

We can print the text inside the page using get_text(); this is shown in the following code snippet:

```
print(soup_packtpage.get_text())
```