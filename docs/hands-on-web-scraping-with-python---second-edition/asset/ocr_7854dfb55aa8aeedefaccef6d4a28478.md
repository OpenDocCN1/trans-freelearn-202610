![](bbox=[0.183, 0.088, 0.755, 0.359])

The `<urlset>` node contains `<url>` child nodes, which contain children such as `<loc>`, `<lastmod>`, `<changefreq>`, and `<priority>`. With distinct nodes in place, we will move through each child `<url>` and collect their children’s information in a list and, finally, write it to a CSV file:

```
```python
from pyquery import PyQuery as pq
import requests, csv
url = "https://www.schools.com/sitemap.xml"
columns=['loc','lastmod','changefreq','priority']
#Columns for CSV header
```
```

The required libraries, the url attribute, and the desired column names (columns) for the CSV format have been declared in the code.

As shown in the following code, a PyQuery object is supplied with an additional argument of `parser='html'`. This allows PyQuery to interpret the nodes and child nodes encountered in the source page as HTML documents:

```
```python
xmlFile = requests.get(url).content  #loading the url
urlXML = pq(xmlFile, parser='html')  #parser
print("Child-Length: ",urlXML.children().__len__())
# Child-Length: 530
```
```