![](bbox=[0.144, 0.108, 0.807, 0.298])

We can also first select the database by executing the `USE <DB>` command, and then we don't have to pass it into the `SHOW TABLES` command because we have already selected the database we want to talk to. The following code creates the same true result as the previous one:

```
```python
cursor.execute("USE guidb")
cursor.execute("SHOW TABLES")
```
```

Now that we know how to verify that our database has no tables, let's create some. After we have created two tables, we will verify that they have truly made it into our database by using the same commands as before.

We create the first table, named *Books*, by executing the following code:

```
```python
# connect by unpacking dictionary credentials
conn = mysql.connect(**guiConf.dbConfig)

# create cursor
cursor = conn.cursor()

# select DB
cursor.execute("USE guidb")

# create Table inside DB
cursor.execute("CREATE TABLE Books (
        Book_ID INT NOT NULL AUTO_INCREMENT,
        Book_Title VARCHAR(25) NOT NULL,
        Book_Page INT NOT NULL,
        PRIMARY KEY (Book_ID)
    ) ENGINE=InnoDB")

# close connection to MySQL
conn.close()
```
```