Like prettify(), encode() also takes a different encoding as its parameter.

```
print(soup.encode("ISO-8859-2")

#output
cédille (from French), is a hook or tail ( ç ) added under certain
letters as a diacritical mark to modify their pronunciation
```

# Quick reference

You can take a look at the following references to get an overview of the code present in this chapter:

+   - `soup = BeautifulSoup(html_markup,"lxml",from_encoding="latin-1")`. Here, from_encoding is used while creating BeautifulSoup to specify the document encoding.
+   - `soup.original_encoding`: This gives the original encoding detected by Beautiful Soup.
+   - The output content in specific encoding is listed using the following methods:
+     - `soup.prettify()`
+     - `soup.encode()`

# Summary

In this chapter, we saw the encoding support in Beautiful Soup. We understood how to get the original encoding detected by Beautiful Soup. We also learned to create a BeautifulSoup object by explicitly specifying the encoding. The output encoding was also discussed in this chapter. The next chapter deals with the different methods provided by Beautiful Soup to display content.