Next, we will create the second table, which will store the book and journal quotations. We will create it by executing the following code:

```
# select DB
cursor.execute("USE guidb")

# create second Table inside DB
cursor.execute("CREATE TABLE Quotations (
    Quote_ID INT,
    Quotation VARCHAR(250),
    Books_Book_ID INT,
    FOREIGN KEY (Books_Book_ID)
        REFERENCES Books(Book_ID)
        ON DELETE CASCADE
) ENGINE=InnoDB")
```

Executing the SHOW TABLES command now shows that our database has two tables:

GUI_MySQL_class.py

![](bbox=[0.205, 0.42, 0.87, 0.62])

We can see the columns by executing the SQL command using Python:

GUI_MySQL_class.py

![](bbox=[0.105, 0.71, 0.965, 0.845])