# Example 1 – scraping book details

In this example, we will be scraping books listed on http://books.toscrape.com in the Childrens category (http://books.toscrape.com/catalogue/category/books/childrens_11/index.html). This example is similar to another provided in Chapter 3, where the lxml library was used. The code for this example is available on GitHub: https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter04/example_1.ipynb.

As you can see in Figure 4.3, the Childrens category contains 29 results (a single page shows only 20 results):

![](bbox=[0.11, 0.3, 0.85, 0.54])

Figure 4.3: Childrens category page with 29 results

# Important note

You are recommended to browse the pages for the selected category using DevTools and find the differences in URLs or content by using the element inspector tools and moving across the page, or exploring the page source and identifying the targeted elements.

After studying the basics of processing pages and URLs, we can move on to the script. The following code defines the required libraries, URLs, default pagination value, and an empty dataSet to collect the data:

```
from pyquery import PyQuery as pq
import requests, math
siteUrl="http://books.toscrape.com/"
baseUrl=siteUrl+"catalogue/category/books/childrens_11/index.html"
pageUrl=siteUrl+"catalogue/category/books/childrens_11/page-"
```