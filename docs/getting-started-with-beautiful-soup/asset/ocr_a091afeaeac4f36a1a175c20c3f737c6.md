```
10000
</div>
</li>
</ul>
</div>
</body>
</html>
```

# Deleting tags from the HTML document

Beautiful Soup also allows for the removal of tags from the document. This is accomplished using the decompose() and extract() methods.

# Deleting the producer using decompose()

We have added the new producer, phytoplankton.

We can remove this producer entry by removing the div tags first and then the li tags from the ul tag. We will remove the div tag with class="name" using the decompose() method.

```
third_producer = soup.find_all("li")[2]
div_name = third_producer.div
div_name.decompose()
print(third_producer.prettify())

#output
<li class_="producerlist">
  <div class_="number">
    10000
  </div>
</li>
```

In the preceding code, we have used find_all() to find all the li entries and we stored the third producer into the third_producer variable. Then, we found the first div tag and removed it using the decompose() method.

> The decompose() method is used to remove the tag from an HTML/XML document. It will remove the tag and its children.