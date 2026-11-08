# Category dataset

The first type of dataset is known as a **category dataset**, where the dataset series and values are grouped by categories. A series is like a subgroup. If the exact category and series appear, the chart will sum the values into a single result. The following table is a simple example of a category dataset:

| Category | Series | Sale price |
|---|---|---|
| Store 1 sales | Cash | 14 |
| Store 1 sales | Credit | 12 |
| Store 2 sales | Cash | 100 |
| Store 2 sales | Credit | 200 |

The charts that use the category dataset are: bar chart, line chart, area chart, bar line chart, waterfall chart, and radar chart.

Pentaho Reporting builds a category dataset using `CategorySetDataCollector`. Also available is `PivotCategorySetCollector`, which pivots the category and series data. Collector classes implement Pentaho Reporting's function API. You can view (and change) the collector from the chart editor, in the data properties panel, as the first drop-down menu in the tabs.

In the following section, you can see listed the set of properties defined to control the category dataset using the chart editor. The properties are grouped in