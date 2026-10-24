
the data from all available `<loc>...</loc>` markups; similarly, `mods` lists the content from all `<lastmod>...</lastmod>` markups:

```python
locs = re.findall(r"<loc>(.*)</loc>", source)
mods = re.findall(r"<lastmod>(.*)</lastmod>", source)
```

`locFinals` is a Python list that contains individual elements from `locs` and `mods` as a tuple. The `zip(locs, mods)` Python function works as a merger that combines the argument passed to it and creates a tuple with the value from iterated `locs` and `mods`:

```python
locFinals = list(zip(locs, mods))
print(locFinals[0])    # (locs[0], mods[0])
('https://www.zyte.com/blog/json-parsing-with-python/', '2023-04-06T14:42:05+00:00')
```

`locFinals` has multiple elements. Iteration is initiated along with unpacking the tuple and cleaning plus splitting the data (using `re.sub()` and `re.split()`) into chunks to retrieve the desired data:

```python
for locFinal in locFinals:
    loc, datetime = locFinal
    # unpacking tuple (extracting tuple elements)
    loc = re.sub('^(http\:|https\:)', '', loc)
    # cleaning
    loc = re.sub('[\/]{2}', '', loc)
    loc = re.sub('\/$', '', loc)
    locSplit = re.split(r"/", loc)
    # breaking
    category = locSplit[1]
    categoryTopic = re.sub('[-]', ' ', locSplit[2])
    ymdTime = re.split("T", datetime)
    # separating date and time
    ymd = ymdTime[0]
    time = re.sub("\+.*", "", ymdTime[1])
    # cleaning
```

The identified desired data is finally loaded into the `dataSet` data container:

```python
dataSet.append(["Zyte", category, categoryTopic, ymd, time])
print(dataSet)
[['Zyte', 'blog', 'json parsing with python', '2023-04-06', '14:42:05'], ...,
 ['Zyte', 'webinars', 'the right data fields for e commerce data project', '2023-03-21', '06:17:35'], ...]
```

As seen in this example, re methods (`findall()`, `sub()`, and `split()`) and the `zip()` Python method play a major role. Regex-based extraction can be done effectively and easily by engaging Python re and in-built methods.

### Example 3 – Godfrey’s dealer

In this example, we will extract dealer details from `https://godfreysfeed.com/dealersandlocations`.

An HTTP request is initiated using the requests Python library, and the response is collected as source:

```python
url = "https://godfreysfeed.com/dealersandlocations"
source = requests.get(url).text
```

From source, we will be obtaining the data related to the desired columns, such as Name, Address, City, State, Zip, Lat, and Lng. Dealer-related data is only available in *JavaScript*. The data is also scattered across multiple lines, and it contains some HTML tags, as seen here:

```javascript
var infoWindowContent = "<div style='overflow:hidden; width:200px'>";
infoWindowContent = infoWindowContent + '<strong><span style='color:#e5011c;'>American Cowboy Shop</span></strong><br><strong>513 D Murphy Hwy</strong><br><strong>Blairsville, GA</strong><br><strong>30512</strong><br><br>';
infoWindowContent += '</div>';
var infowindow = new google.maps.InfoWindow({maxWidth: 450});
infowindow.setContent(infoWindowContent);
var latLng = new google.maps.LatLng(34.8752421, -83.9716038);
```

The regex is defined to collect all coordinate data with the `latlngs` variable, and `contents` holds the address-carrying `infoWindowContent` variable. Finally, `details` is created using Python’s built-in `zip()`:

```python
latlngs = re.findall(r"var\s+latLng.*\.LatLng\((.*)\);", source)
contents = re.findall(r"infoWindowContent\+\s*\"(.*)\";", source)
details = list(zip(contents, latlngs))  # zip
```

Now, we have identified `latlngs` and `contents` data in `details`. Iteration in `details` is required, which unpacks the `detail` tuple into a block of `fullAddress` and `coordinate`:

```python
for detail in details:
    fullAddress, coordinate = detail
```

to know that *Word documents*, *PowerPoint presentations*, *HTML*, *Jupyter notebooks*, *analysis reports from various applications*, and many more content types support exporting and saving files as PDF.

We often find various types of data (such as textual, tabular, and images) in a PDF file. In *Chapters 3 and 4*, we saw how to extract web-based content using Python. Here, we will be using the PyPDF2 Python library to extract data from PDF files.

In the next sections, we will install and explore PyPDF2 from a data extraction perspective.

## The PyPDF2 library

PyPDF2 (https://pypdf2.readthedocs.io/en/3.0.0/index.html) is a free, open source (https://github.com/py-pdf/PyPDF2) Python library to work with PDF files and is quite famous among Python developers and the community because of its various features.

Listed here are a few of its major functionalities:

- Text extraction
- Image extraction
- Metadata (document details) availability and extraction
- File conversion (PDF to Word, and more)
- Text modification (PDF files)
- Adding a watermark to existing or new PDF files
- Adding security, such as password-protecting PDF files
- Splitting a PDF file into pages
- Merging PDF files into one PDF
- Dealing with the page layout, cropping, transformation, and orientation (such as rotating the page)

It should also be noted that PDF files can be created using PDF applications or exported and saved from another document format. Though PDF files contain the proper formatting of the original document, their content is not like HTML or markup-based documents. PDF contents are available in raw format (with line breaks, spaces, and tabs), and the use of regex is almost compulsory to target and extract the desired information.

> **Important note**
> There are plenty of Python libraries that deal with PDF files, such as `pdfminer`, `pdfquery`, `xpdf`, `pdfrw`, `pikepdf`, and `pymupdf`. PyPDF2 is my favorite among those listed because of its various features. As explained at https://pypdf2.readthedocs.io/en/3.0.0/user/installation.html, PyPDF2 can be used for broad use cases, or can just be focused on a domain, such as *crypto (encrypting or decrypting)* and *image processing*.

In the next section, we will use PyPDF2 to collect data.

## Extraction using PyPDF2

PyPDF2 is a complete package; there are various classes to deal with its different features. We will be dealing with the `PdfReader` class (https://pypdf2.readthedocs.io/en/3.0.0/modules/PdfReader.html) from PyPDF2, for extraction purposes.

As seen in the following code, for extraction, the `PdfReader` class needs to be imported from PyPDF2. The target file or file path (`pdffile`) is provided to `PdfReader`. `reader` is an object that carries methods and attributes from `PdfReader`:

```python
from PyPDF2 import PdfReader
pdffile = "python_cheat_sheet_v1.pdf" # file path or name
reader = PdfReader(pdffile)
# reader = PdfReader(open(pdffile,'rb'))
dir(reader)
['__class__',..., '__encryption__',...'isEncrypted', 'is_encrypted',
 'metadata', 'namedDestinations', 'named_destinations', 'numPages',
 'outline', 'outlines', 'pageLayout', 'pageMode', 'page_layout',
 'page_mode', 'pages', 'pdf_header', 'read', 'readNextEndLine',
 'readObjectHeader', 'read_next_end_line', 'read_object_header',
 'resolved_objects', 'stream', 'strict', 'threads', 'trailer',...]
```

PyPDF2 recently received an update that equipped it with new methods and attributes. There are also deprecated entities, which you might come across when exploring PyPDF2.

The number of pages in a PDF can be found by using `len(reader.pages)`, and individual page sources or content can be targeted with `page1 = reader.pages[0]` (here, index 0 refers to page 1, 1 to page 2, and so on). With the page number identified, content can be extracted using the `extract_text()` method:

```python
page1 = reader.pages[0]    # point to first page
page1_source = page1.extract_text()    # extract from first page
len(page1_source)                # 964
page1_source[:100]  # Python is a beautiful language. It's easy to learn and fun, and its syntax is simple yet elegant. Pyt
page1_source
"Python is a beautiful language. It's easy to learn and fun, and its syntax is simple yet elegant. ..... ...... Python Cheat Sheet\n1. Primitives Numbers"
```

The preceding code loads the PDF file content or pages using PyPDF2. In the examples discussed next, we will be applying regex to extract the desired content from the PDF.
