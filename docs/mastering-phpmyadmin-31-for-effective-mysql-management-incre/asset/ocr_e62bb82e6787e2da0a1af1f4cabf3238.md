This time, there are buttons labeled <<, <, >, and >> for easy access to the first page, previous page, next page, and the last page of the results respectively. The buttons appear only when necessary; for example, the first page button is not displayed if we already are on the first page. These symbols are displayed in this manner, as the default setting of $cfg['NavigationBarIconic'] is TRUE. A FALSE here would produce a different set of labels.

![](bbox=[0.42, 0.25, 0.58, 0.31])

There is also a Page number drop-down menu, to go directly to one of the pages located near the current page. As there can be hundreds or thousands of pages, this menu is kept small, with only a few page numbers before and after the current page.

By design, phpMyAdmin always tries to give quick results, and one way to achieve this result is to add a LIMIT clause in SELECT. If a LIMIT clause is already there in the original query, phpMyAdmin will respect it. The default limit is 30 rows, set in $cfg['MaxRows']. The multiple users on the server help keep the server load to a minimum.

Another button is available on the navigation bar, but must be activated by setting $cfg['ShowAll'] to TRUE. It would be very tempting for users to use this button often. Hence, on a multi-user installation of phpMyAdmin, it is recommended that the button be disabled (FALSE). When enabled, the navigation bar is augmented with a Show all button. Clicking on this button retrieves all the rows of the current results set, which might hit the execution time limit in PHP or a memory limit in the server or browser.

> If we enter a big number in the Show...rows dialog, the same results will be achieved (and we may face the same problems).

# Query results operations

A section labeled Query results operations is located under the results. It contains links to print the results (with or without the FULL TEXT columns), to export these results (see Exporting Partial Query Results section in Chapter 7), or to create a view from this query (more on this in Chapter 17).