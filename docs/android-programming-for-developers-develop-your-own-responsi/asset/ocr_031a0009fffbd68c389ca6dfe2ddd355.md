Note that in the previous code, the regular Java variables name and score are highlighted. The previous String called query is now the SQL statement that is exactly equivalent to the following code:

```
INSERT INTO StudentsAndGrades (
 name, score)
 VALUES ('Divij',94);
```

# Tip

It is not essential to completely grasp the previous two blocks of code in order to proceed with learning Android programming. But if you want to build your own apps and construct SQL statements that do exactly what you need, it will help to do so. Why not study the previous two blocks of code in order to discern the difference between the pairs of double quote marks " that are the parts of the String joined together with +, the pairs of single quote marks ' that are part of the SQL syntax, the regular Java variables, and the distinct semicolons from the SQL statement in the String and Java.

Throughout the typing of the query, Android Studio prompts us the names of our variables, making the chances of an error much less likely even though it is more verbose than simply typing the query.

Now, we can use the classes that we introduced previously to execute the query:

```
// This is the actual database
private SQLiteDatabase db;

// Create an instance of our internal CustomSQLiteOpenHelper class
CustomSQLiteOpenHelper helper = new
CustomSQLiteOpenHelper(context);

// Get a writable database
db = helper.getWritableDatabase();

// Run the query
db.execSQL(query);
```

When adding data to the database, we will use execSQL, as in the previous code, and when getting data from the database, we will use the rawQuery method, as shown in the next code snippet:

```
Cursor c = db.rawQuery(query, null);
```