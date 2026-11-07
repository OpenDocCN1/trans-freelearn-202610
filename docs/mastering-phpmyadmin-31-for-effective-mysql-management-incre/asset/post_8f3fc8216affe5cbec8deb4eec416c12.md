
To demonstrate that the OR operator links both the criteria rows better, let's now add a condition, LIKE '%8%', on the phone number.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_236_0.png)

By examining the positioning of the AND and OR operators, we can see that the first row of the conditions is linked by the AND (because AND is chosen under the name column) operator, and the second row of conditions is linked to the rest by the OR operator. The condition we've just added (LIKE '%8%') is not meant to find anyone, since, we changed the phone number of all the authors with the name "Smith" to "444-5555" (in Chapter 12).

If we want another criterion on the same column, we just add a criteria row.

## Adjusting the number of criteria rows

The number of criteria rows can be changed in two ways. First, we can select the Ins checkbox under Criteria to add one criteria row (after clicking on Update Query). As this checkbox can add only one criteria row at a time, we'll uncheck it and use the Add/Delete Criteria Row dialog instead. In this dialog, we choose to add two rows.

Another click on the **Update Query** button produces the following screen:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_237_0.png)

Now, you can see that there are two additional criteria rows (which are empty at the moment). We can also remove criteria rows. This can be done by choosing negative numbers in the **Add/Delete Criteria Row** dialog, or by ticking the **Del** checkbox beside the row(s) we want to remove. Let's remove the two rows we've just added, as we don't need them now. The **Update Query** button refreshes the page with the specified adjustment.

## Adjusting the number of criteria columns

Using a similar mechanism, we can add or delete columns by checking the INS or Del checkboxes under each column, or the Add/Delete Field Columns dialog. We already had one unused column (not shown on the previous images to save space). Here, we have added one column using the Ins checkbox located under the unused column (this time, we will need it):

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_238_0.png)

## Automatic joins

phpMyAdmin can generate the joins between the tables in the query it builds. Let's now populate our two unused columns with the title and the genre fields from our book table, and see what happens when we update the query.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_238_1.png)

There are now two additional criteria columns that relate to the `book`.`title` and the `book`.`genre` columns respectively. phpMyAdmin used its knowledge of the relations defined between the tables to generate a LEFT JOIN clause (highlighted in the previous figure) on the author_id key field. A shortcoming of the current version is that only the internal relations, and not the InnoDB relations, are examined.

> There may be more than two tables involved in a join.

## Executing the query

Clicking the Submit Query button sends the query for execution. In the following figure, you can see the complete generated query in the upper part and the resulting data row in the lower part:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_239_0.png)

There is no easy way (except by using the browser's **Back** button) to come back to the query generation page once we have submitted the query. The next chapter discusses how to save the generated query for later execution.

## Summary

This chapter covers various aspects including opening the query generator, choosing tables, entering column criteria, sorting and showing columns, and altering the number of criteria rows or columns. It also shows us how to use the AND and OR operators to define relations between the rows and columns, and how to use automatic joins between tables. Chapter 14 will show you how to keep permanent bookmarks of your queries.

## 14 Bookmarks

This chapter covers query bookmarks — one of the features of the linked-tables infrastructure. Being able to label queries, and recall them by label, can be a real time saver. Bookmarks are queries that are:

- Stored permanently
- Viewable
- Erasable
- Related to one database
- Recorded only as a consequence of a user's wish
- Labeled
- Private by default (only available to the user creating them), but possibly public

A bookmark can also have a variable part, as explained in the Passing a parameter value to a bookmark section later in this chapter.

There is no bookmark subpage to manage bookmarks. Instead, the various actions on bookmarks are available on specific pages, such as results pages or query box pages.

## Comparing bookmark and query history features

In Chapter 12, we learned about the SQL history feature, which automatically stores queries (temporarily or permanently). There are similarities between queries stored in the history and bookmarks. After all, both features are intended to store queries for later execution. However, there are important differences, regarding the way they are stored and the action that triggers the recording of a query.

There is a configurable limit (see Chapter 12) on the number of queries stored in the permanent history; however, the number of bookmarks is not limited. Also, the history feature presents the queries in the reverse order of the time they were sent. However, the bookmarks are shown by label (not showing the query text directly). Finally, query storing in the history is automatic, whereas a query is saved as a bookmark via an explicit request from the user.

To summarize, the automatic query history is useful when we neither plan to recall a query, nor wish to remember which queries we typed. This contrasts with the bookmark facility where we intentionally ask the system to remember a query, even giving it a name (label). Therefore, we can do more with bookmarks than with the query history, but both features have their own importance.

## Bookmark creation

There are two moments when it is possible to create a query – after a query is executed (in this case we don't need to plan ahead its creation) or before sending the query to the MySQL server for execution. Both these options are explored in the following sections.

## Creating a bookmark after a successful query

Initial bookmark creation is made possible by the Bookmark this SQL query button. This button appears only after executing a query that generates a result (when at least one row is found); so this method for creating bookmarks only stores SELECT statements. For example, a complex query produced by the multi-table query generator (as seen in Chapter 13) could be stored as a bookmark this way, provided it finds some results.

Let's see an example. In the Search page for the book table, we select the fields that we want in the results, and enter the search values as shown in the following screenshot:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_243_0.png)

After clicking Go, we see that the results page shows a bookmark dialog. We enter only a label for this bookmark, and click Bookmark this SQL query to save this query as a bookmark. Bookmarks are saved in the table defined by `$cfg['Servers'][$i]['bookmarktable']`.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_243_1.png)

This bookmark dialog can be seen on any page that contains results. As a test, we could just click **Browse** for a table to get results, and then store this query as a bookmark. However, it does not make much sense to store (in a bookmark) a query that can easily be made with one click.

## Storing a bookmark before sending a query

Sometimes, we may want to store a bookmark even if a query does not find any results. This may be the case if the matching data is not yet present, or if the query is not a SELECT statement. To achieve this, we have the **Bookmark this SQL query** dialog available in the **SQL** tab of the Database view, Table view, and the query window.

We now go to the **SQL** subpage of the **book** table, enter a query, and directly put the **books in French** bookmark label in the **Bookmark this SQL query** field. If this bookmark label has been used previously, a new bookmark with the same name will be created unless we select the **Replace existing bookmark of same name** checkbox. Bookmarks carry an internal identifying number, as well as a label chosen by the user.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_244_0.png)

On clicking **Go**, the query is executed and stored as a bookmark. It does not matter if the query does not find anything. This is how we can generate bookmarks for non-SELECT queries such as UPDATE, DELETE, CREATE TABLE, and so on.
