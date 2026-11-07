There is a configurable limit (see Chapter 12) on the number of queries stored in the permanent history; however, the number of bookmarks is not limited. Also, the history feature presents the queries in the reverse order of the time they were sent. However, the bookmarks are shown by label (not showing the query text directly). Finally, query storing in the history is automatic, whereas a query is saved as a bookmark via an explicit request from the user.

To summarize, the automatic query history is useful when we neither plan to recall a query, nor wish to remember which queries we typed. This contrasts with the bookmark facility where we intentionally ask the system to remember a query, even giving it a name (label). Therefore, we can do more with bookmarks than with the query history, but both features have their own importance.

# Bookmark creation

There are two moments when it is possible to create a query – after a query is executed (in this case we don't need to plan ahead its creation) or before sending the query to the MySQL server for execution. Both these options are explored in the following sections.

# Creating a bookmark after a successful query

Initial bookmark creation is made possible by the Bookmark this SQL query button. This button appears only after executing a query that generates a result (when at least one row is found); so this method for creating bookmarks only stores SELECT statements. For example, a complex query produced by the multi-table query generator (as seen in Chapter 13) could be stored as a bookmark this way, provided it finds some results.

Let's see an example. In the Search page for the book table, we select the fields that we want in the results, and enter the search values as shown in the following screenshot: