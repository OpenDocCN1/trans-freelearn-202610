
As seen in the following code, the book_details.json dataset is being loaded to a pandas DataFrame using read_json(). The ProfileReport class of ydata_profiling accepts the books DataFrame and creates a report with the provided title attribute.

Here, the report generated has been exported to an HTML file named book_details_rawdata.html:

```
import pandas as pd
from ydata_profiling import ProfileReport
books = pd.read_json("book_details.json")# input
books_profile = ProfileReport(books,
    title="Book Details - Raw Data , Report")

books_profile.to_file("book_details_rawdata.html")# output
```

The generated book_details_rawdata.html file will have various menu options, as shown in Figure 10.5. This report file can also be opened in a new browser tab:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_254_0.png)

Figure 10.5: EDA – HTML report

Moving through the report menus, we can also find detailed statistical descriptions for data variables, as shown in Figure 10.6. Depending on the type (numerical, categorical, and others) of the variable, detailed analysis reports will vary.

## Quantile statistics

## Descriptive statistics

| Minimum | 1 |
| 5-th percentile | 1.4 |
| Q1 | 3 |
| median | 7 |
| Q3 | 14 |
| 95-th percentile | 17.2 |
| Maximum | 19 |
| Range | 18 |
| Interquartile range (IQR) | 11 |

| Standard deviation | 5.8575033 |
| Coefficient of variation (CV) | 0.74177989 |
| Kurtosis | -1.3381785 |
| Mean | 7.8965517 |
| Median Absolute Deviation (MAD) | 4 |
| Skewness | 0.52015565 |
| Sum | 229 |
| Variance | 34.310345 |
| Monotonicity | Decreasing |

Figure 10.6: Statistical description in an EDA report

As mentioned in the introduction to this section, the cleanliness and accuracy of the analysis are based on the data we are using. For this chapter, book data scraped from http://books.toscrape.com/ has been used. Two EDA reports have been made and are available in this chapter's GitHub repository: one uses the raw dataset, and another uses a cleaned data file.

### Important note
ydata_profiling is an updated version of the existing pandas_profiling library. As mentioned in https://pypi.org/project/pandas-profiling/, pandas_profiling will be outdated soon. There is also another library named dataprep from https://dataprep.ai/. dataprep provides more customizable and granular reports than pandas_profiling.

Despite their ease of use and the lack of long code, output in EDA reports is limited and might not be suitable for in-depth analysis of data variables. In such cases, coding and performing manual analysis is compulsory, which we will be doing in the next section.

## pandas and plotly

pandas is one of the most widely used data analysis libraries. Performance, data structure (series, DataFrames), ease of use, convertibility, input-output compatibility, availability of features, and many other things keep pandas at the top of its category. For detailed information on pandas, please visit https://pandas.pydata.org/.

plotly, on the other hand, is a graphing library (https://plotly.com/python/). It contains some advanced imaging features such as downloading images in .png format, zooming in and out, auto-scaling, pan clicking and dragging, resetting axes, and more. These plotly features are available in the top right-hand corner of visualizations without using code, as shown in Figure 10.7:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_256_0.png)

Figure 10.7: Plotly image options

pandas can be used with various graphing libraries, such as matplotlib and seaborn. The plot() method from pandas can be used directly on top of data. pandas also provides options to choose graphing libraries and apply them as required in code or notebooks. For example, pd.options.plotting.backend = "plotly" sets pandas’ plotting library to plotly:

```
import pandas as pd
import numpy as np
import plotly.express as px
pd.options.plotting.backend = "plotly"
```

Since Jupyter notebooks are used in code examples, we will find the preceding code at the top of the notebook. This code can also be called or imported in the required places.

Let's explore some basic usage of pandas and analyze data through some examples.

## Example 1 – book analysis

The code for this example is available at data_analysis_book.ipynb.

The read_json() method reads the JSON file and creates a DataFrame called books. The shape attribute returns a tuple object where the first value is the number of rows and the last value is the number of columns. describe() displays statistical details about the numerical columns. The include="all" argument in describe() provides the statistical detail of all available columns or those returned from books.columns:

```
books = pd.read_json("book_details.json")
books.shape            # (29, 8)
books.describe()
books.describe(include="all")
books.info

books.columns
Index(['Upc', 'Title', 'Price', 'Rating', 'Stock', 'Stock_Qty', 'Url', 'Image'], dtype='object')
```

Column names can be used as attributes, such as books.Title, or in indexed format, such as books['Title']. Python supports indexing in [START: STOP: STEP] syntax. In books.Title[::3], every third book title is provided:

```
books.Title[::3]   # no start an stop, only step
0       Birdsong: A Story in Pictures
3        The White Cat and the Monk: A Retelling of the...
......
24                              Counting Thyme
27                              Matilda
Name: Title, dtype: object
```

There are various methods and attributes in pandas for accessing row data. The most common one is index and location, or iloc[]:

```
books.iloc[2]                # returns row from index 3 (0,1,2)
Upc                                 b5ea0b5dabed25a8
Title                       The Secret of Dreadwillow Carse
......
Url     http://books.toscrape.com/catalogue/the-secret...
Image   http://books.toscrape.com/media/cache/c4/a2/c4...
Name: 2, dtype: object
```

Checking unique values and counting the total number of unique values is quite applicable during analysis. Here, books has 29 rows, but there are only 5 unique values (nunique() returns the number of unique elements). This suggests that duplicate values exist in the Rating column. Rating values are strings, which might cause problems if any statistical computation is required. Therefore, cleaning is required for the Rating column:

```
books["Rating"].unique()
# array(['Three', 'One', 'Four', 'Five',  'Two'], dtype=object)
books["Rating"].nunique() # 5
```

One of the important aspects of using pandas is data filtering. Various methods can be used for filtering, such as filter(), where(), and query(). Also, as shown in the preceding code, logical operations (such as and and or) can be used for filtering. [['Title',  'Price',  'Stock_Qty']], after the query() operation, collects and displays results from only the three columns mentioned:

```
books.query("Stock_Qty >= 15 and Title.str.contains('Bear')")
[['Title', 'Price', 'Stock_Qty']]
```

Similar to the str.contains() method, there are also startswith() and endswith() methods. These methods play crucial roles when dealing with string types, and again when filtering techniques are applied.

As shown in the preceding code, the value_counts() method counts the total number of occurrences of the provided value for the selected columns:

```
ratingCount = books["Rating"].value_counts()
```

The ratingCount variable contains the counts related to book rating and plots the bar chart, as shown in the following code:

```
ratingCount.plot.bar(title="Rating Count", labels=dict(index="Rating", value="Count", variable="Detail"))
```

plotly support various types of chart, such as bar, line, barh, and pie.

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_258_0.png)

Figure 10.8: Using a plotly bar chart

The following code plots a default plot, which is a line chart, with the labels provided:

```
ratingCount.plot(title="Rating Count", template="simple_white", labels=dict(index="Rating", value="Rating_Count", variable="Legends"))
```

If no type is mentioned, plotly will draw a line chart, as shown in Figure 10.9:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_258_1.png)

Figure 10.9: Line chart (ratingCount)
