After inserting data into the books and quotations tables, if we execute a `DELETE` statement, we are only deleting the book with `Book_ID 1` while the related quotation with the `Books_Book_ID 1` is left behind.

This is an *orphaned record*. There no longer exists a book record that has a `Book_ID` of 1:

```
mysql> SELECT * FROM books;
+---------+----------------------+-----------+
| Book_ID | Book_Title           | Book_Page |
+---------+----------------------+-----------+
|       2 | xUnit Test Patterns  |        31 |
+---------+----------------------+-----------+
1 row in set (0.00 sec)

mysql> SELECT * FROM quotations;
+---------+------------------------------------------+----------------+
| Quote_ID | quotation                                | Books_Book_ID |
+---------+------------------------------------------+----------------+
|       1 | programming to an Interface, not an Implementation |              2 |
|       2 | philosophy of Test Automation             |              2 |
+---------+------------------------------------------+----------------+
2 rows in set (0.00 sec)

mysql>
```

This situation can create a mess, which we avoid by using cascading deletes.

We do this in the creation of the tables by adding certain database constraints. When we created the table that holds the quotations in a previous recipe, we created our `quotations` table with a foreign key constraint that explicitly references the primary key of the books table, linking the two:

```
# create second Table inside DB
cursor.execute("CREATE TABLE Quotations (
            Quote_ID INT AUTO_INCREMENT,
            Quotation VARCHAR(250),
            Books_Book_ID INT,
            PRIMARY KEY (Quote_ID),
            FOREIGN KEY (Books_Book_ID)
                REFERENCES Books(Book_ID)
                ON DELETE CASCADE
        ) ENGINE=InnoDB")
```

The `FOREIGN KEY` relation includes the `ON DELETE CASCADE` attribute, which basically tells our MySQL server to delete related records in this table when the records that this foreign key relates to are deleted.