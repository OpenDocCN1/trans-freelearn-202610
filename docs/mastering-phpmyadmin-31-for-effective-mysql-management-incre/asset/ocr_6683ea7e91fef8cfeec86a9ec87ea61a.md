This bookmark dialog can be seen on any page that contains results. As a test, we could just click **Browse** for a table to get results, and then store this query as a bookmark. However, it does not make much sense to store (in a bookmark) a query that can easily be made with one click.

# Storing a bookmark before sending a query

Sometimes, we may want to store a bookmark even if a query does not find any results. This may be the case if the matching data is not yet present, or if the query is not a SELECT statement. To achieve this, we have the **Bookmark this SQL query** dialog available in the **SQL** tab of the Database view, Table view, and the query window.

We now go to the **SQL** subpage of the **book** table, enter a query, and directly put the **books in French** bookmark label in the **Bookmark this SQL query** field. If this bookmark label has been used previously, a new bookmark with the same name will be created unless we select the **Replace existing bookmark of same name** checkbox. Bookmarks carry an internal identifying number, as well as a label chosen by the user.

![](bbox=[0.17, 0.46, 0.83, 0.62])

On clicking **Go**, the query is executed and stored as a bookmark. It does not matter if the query does not find anything. This is how we can generate bookmarks for non-SELECT queries such as UPDATE, DELETE, CREATE TABLE, and so on.