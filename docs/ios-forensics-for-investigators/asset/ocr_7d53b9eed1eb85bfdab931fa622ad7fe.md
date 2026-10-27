the offset of each frame, isolate each individual frame, and view the data by converting it to a UTF-8 string to identify any readable strings.

As an example, we're going to create a new database named test.db that contains a single table called Fruits. Then, we'll populate the database with a few records, delete a row of data, and analyze the file with a hex viewer. For simplicity, the database will not use write-ahead logging.

You can create the database using your software of choice or the sqlite3 command-line tool. Then, run the following queries:

```
CREATE TABLE "Fruits" (
    "ROWID"    INTEGER,
    "Fruit"    TEXT,
    PRIMARY KEY("ROWID" AUTOINCREMENT)
);
INSERT INTO "Fruits" VALUES (1,'Apple');
INSERT INTO "Fruits" VALUES (2,'Peach');
INSERT INTO "Fruits" VALUES (3,'Orange');
INSERT INTO "Fruits" VALUES (4,'Banana');
DELETE FROM Fruits WHERE ROWID == 1;
```

As you can see from the following screenshot, the table now contains three rows, as expected:

![](bbox=[0.25, 0.59, 0.74, 0.77])

Figure 4.12 – The table contains three rows of data

The first step in our examination is to open the file with a hex viewer and analyze the 100-byte header to extract any useful information from the database: