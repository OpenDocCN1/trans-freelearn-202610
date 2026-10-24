In this section, we have covered some important techniques and methods that are used for file handling.

In the next sections, we will explore some structured types of files and look at how to read and write to them.

# JSON

**JavaScript object notation (JSON)** is a structured file format. From web APIs to local file content, the utilization of JSON is on the rise due to its string-based features and resemblance to Python. JSON contents are made from a mixture of *dict()* and *list()*. Python’s built-in `json` library can be used to manage JSON files. There are various ways to read files; we can use Python file-handling techniques, as shown in the following code:

```
import json
with open("book_details.json", "r") as file:
    books = json.loads(file.read())
    print(books[0]['Title'])
    Birdsong: A Story in Pictures

    print(books[0])
    {'Upc': '9528d0948525bf5f', 'Title': 'Birdsong: A Story
    in Pictures', 'Price': '£54.64',
    'Rating':'http://books.toscrape.com/media/cache/af/6e/
    af6e2.jpg'}
```

The `json` method loads content using the `loads()` method and the content is returned by the `file` object using the `read()` method. Once the JSON content is loaded, it can be accessed and used as a Python dictionary by using keys and indexes.

In the following sections, we’ll be dealing with basic read and write activities with JSON files.

# *Reading JSON files*

Today’s web content is more and more JSON-driven, and JSON has been an effective part of storing and analyzing data. The pandas Python library is used to analyze data, and it also provides a method called `read_json()` for reading JSON files. It converts JSON content into DataFrame format with a row and column (almost tabular) structure. There is also an attribute named shape, which returns the shape (rows, columns) of the DataFrame, as shown in the following code block:

```
import pandas as pd
bookJson = pd.read_json("book_details.json")
print(type(bookJson))
pandas.core.frame.DataFrame
print(bookJson.shape)
(29,8)
```