```
// Delete a record
public void delete(String name){

    // Delete the details from the table if already exists
    String query = "DELETE FROM " + TABLE_N_AND_A +
    " WHERE " + TABLE_ROW_NAME +
    " = '" + name + "';";

    Log.i("delete() = ", query);

    db.execSQL(query);
}
```

Next, we have the selectAll method that also does as its name suggests. It achieves this with a SELECT query using the * parameter, which is equivalent to specifying all the columns individually. Also note that the method returns Cursor, which we will use in the MainActivity class.

Add the selectAll method to the DataManager class:

```
// Get all the records
public Cursor selectAll() {
    Cursor c = db.rawQuery("SELECT * "+" from " +
    TABLE_N_AND_A, null);

    return c;
}
```

Now, we add a searchName method, which has a String parameter for the name the user wants to search for. It also returns Cursor, which will contain all the entries that were found. Note that the SQL statement uses SELECT, FROM, and WHERE to achieve this:

```
// Find a specific record
public Cursor searchName(String name ) {
    String query = "SELECT " +
    TABLE_ROW_ID + ", " +
    TABLE_ROW_NAME +
    ", " + TABLE_ROW_AGE +
    " from " +
    TABLE_N_AND_A + " WHERE " +
    TABLE_ROW_NAME + " = '" + name + "';";

    Log.i("searchName() = ", query);
```