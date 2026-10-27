# Tables, columns, and rows

The basic concepts of an SQLite database should be easy to visualize – a database contains one or more **tables**, which each contain data that is organized into **columns** and **rows**. A column represents a single data element, such as a *phone number* or *address*, while a row represents a set of values, one for each column.

SQLite supports five different data types:

+   - INTEGER
+   - TEXT
+   - REAL
+   - BLOB
+   - NULL

The first three types are self-explanatory. The **Binary Large Object (BLOB)** data type preserves any input exactly as is, without casting it as a certain type; BLOB data types are commonly used to store complex binary data structures such as **property lists** or **protocol buffers** in an SQLite record. The NULL data type simply stores an empty value.

To retrieve data from a database, you run a **query**, which is a set of logical instructions written in **SQL language** that parses through a given table to extract the data you want. The result of the query is actually represented as another table, as you can see in the following screenshot.

To understand what a query looks like, let's see a simple example. The following query extracts the last five messages from the sms.db database:

```
SELECT ROWID, text FROM message
ORDER BY ROWID DESC
LIMIT 5;
```

The output can be seen in the following screenshot: