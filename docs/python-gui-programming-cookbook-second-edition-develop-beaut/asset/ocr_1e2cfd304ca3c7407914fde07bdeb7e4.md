Running the preceding code creates the following tables and data in the database we created:

```
mysql> USE guidb
Database changed
mysql> SELECT * FROM books;
+---------+----------------------------------+----------+
| Book_ID | Book_Title                       | Book_Page |
+---------+----------------------------------+----------+
|       1 | Design Patterns                  |        7 |
|       2 | xUnit Test Patterns              |       31 |
+---------+----------------------------------+----------+
2 rows in set (0.10 sec)

mysql> SELECT * FROM quotations;
+----------+-----------------------------------------------+---------------+
| Quote_ID | Quotation                                     | Books_Book_ID |
+----------+-----------------------------------------------+---------------+
|        1 | Programming to an Interface, not an Implementation |             1 |
|        2 | Philosophy of Test Automation                 |             2 |
+----------+-----------------------------------------------+---------------+
2 rows in set (0.00 sec)

mysql>
```

# How it works...

We created a MySQL database, connected to it, and then created two tables that hold the data for a favorite book or journal quotation.

We distributed the data between two tables because the quotations tend to be rather large while the book titles and book page numbers are very short. By doing this, we can increase the efficiency of our database.

In SQL database language, separating data into separate tables is called normalization.

# Using the SQL UPDATE command

This recipe will use the code from the previous recipe, Using the SQL INSERT command, explain it in more detail, and then extend the code to update our data.