| _ID | name | score |
| --- | --- | --- |
| 1 | Bart | 23 |
| 2 | Lisa | 100 |

# # Retrieving data from the database

Here is how we would access all the rows and columns from our database:

```
SELECT * FROM StudentsAndGrades;
```

The previous code asks for every row and column. The * symbol can be read as all.

We can also be a little more selective as this code demonstrates:

```
SELECT score FROM StudentsAndGrades
where name = "Lisa";
```

The previous code would only return 100, which, of course, is the score associated with the name Lisa.

# # Updating the database structure

We can even add new columns after the table has been created and data has been added. This is simple as far as the SQL is concerned but can cause some issues with regard to users' data on already published apps. We will explore this problem more in Chapter 26, Upgrading SQLite – Adding Locations and Maps.

The next statement adds a new column called age that is of the int type:

```
ALTER TABLE StudentsAndGrades
ADD
age int;
```

There are many more data types, keywords, and the ways to use them than we have seen so far. We will see many of them in the two database-driven apps that we will build. This was just a taste. Next, let's look at the Android SQLite API, and now we will begin to see how we can use our new SQLite skills.