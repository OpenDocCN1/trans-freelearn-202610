# Selective exports

At various places in phpMyAdmin's interface, we can export the results that we see, or select the rows that we want to export.

# Exporting partial query results

When results are displayed from phpMyAdmin (here, the results of a query asking for the books from author_id 2), an Export link appears at the bottom of the page.

![](bbox=[0.188, 0.298, 0.81, 0.59])

Clicking on this link brings up a special export panel containing the query on the top, along with the other table export options. An export produced via this panel would contain only the data from this result set.

> The results of single-table queries can be exported in all the available formats, while the results of multi-table queries can be exported only in the CSV, XML, and LaTeX formats.