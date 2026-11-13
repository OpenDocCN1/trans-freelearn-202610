# # What is a database?

A database is both a place of storage and the means to retrieve, store, and manipulate data. It helps to visualize a database before learning how to use it. The actual structure of the internals of a database varies greatly depending upon the database in question. SQLite actually stores all its data in a single file.

It will aid our comprehension greatly; however, if we visualize our data as if it were in a spreadsheet or sometimes, multiple spreadsheets, our database, just like a spreadsheet, will be divided into multiple columns that represent different types of data and rows that represent entries in the database.

Think about a database with names and exam scores. Take a look at the following visual representation of this data to understand how a database would look:

| _ID | name | score |
| --- | --- | --- |
| 1 | Bart | 23 |
| 2 | Lisa | 100 |
| 3 | Jim | 66 |

Note, however, that there is an extra column of data here: an _ID column. We will talk more about this as we proceed. This single spreadsheet-like structure is called a table. As mentioned before, there might be, and often are, multiple tables in a database. Each column of the table will have a name that can be referred to when we speak about the database.