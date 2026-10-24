urlXML refers to the `<urlset>` node and urlXML.children() refers to the `<url>` child nodes of `<urlset>`, which total 530.

Python provides the very useful range(start,stop,step) function. This provides us with the difference between start and stop (0 and 530 in our code). As there are a total of 530 targeted `<url>` elements, a loop has been initiated by providing the index number as eq(loop):

```
```python
dataSet=[]
loops = range(0, urlXML.children().__len__())
# range(start, stop)
for loop in loops:
    child = urlXML.children().eq(loop)  #0, 1, 2, 3..,529
    dataSet.append([
        child.find('loc').text(),
        child.find('lastmod').text(),
        child.find('changefreq').text(),
        child.find('priority').text()
    ])
```
```

With the respective `<url>` instances identified, their `<loc>`, `<lastmod>`, `<changefreq>`, and `<priority>` values are appended to dataSet.

The following code defines a function that is also used in *Chapter 3*. It accepts arguments such as data, or in our case, the collection object, dataSet; the filename to be created; and the column list for the first row of the CSV file (columns) as follows:

```
```python
def writeto_csv(data,filename,columns):
    with open(filename,'w+',newline='',encoding="UTF-8") as
        file:
            writer = csv.DictWriter(file, fieldnames=columns)
            writer.writeheader()
            writer = csv.writer(file)
            for element in data:
                writer.writerows([element])
```
```