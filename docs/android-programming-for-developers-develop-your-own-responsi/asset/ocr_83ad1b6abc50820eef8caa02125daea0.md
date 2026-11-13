```
return c;
}
```

Finally, we get to our inner class, CustomSQLiteOpenHelper, which extends SQLiteOpenHelper. As in the last chapter, we have the same simple constructor, but the onCreate method builds and executes two queries to create both of our required tables.

The onUpgrade method will be left blank until Chapter 26, Upgrading SQLite – Adding Locations and Maps.

Add the CustomSQLiteOpenHelper class as an inner class of DataManager:

```
// This class is created when our DataManager is initialized
private class CustomSQLiteOpenHelper extends SQLiteOpenHelper {
    public CustomSQLiteOpenHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }
}

// This method only runs the first time the database is created
@Override
public void onCreate(SQLiteDatabase db) {
    // Create a table for photos and all their details
    String newTableQueryString = "create table "
        + TABLE_PHOTOS + " ("
        + TABLE_ROW_ID
        + " integer primary key autoincrement not null,"
        + TABLE_ROW_TITLE
        + " text not null,"
        + TABLE_ROW_URI
        + " text not null,"
        + TABLE_ROW_TAG1
        + " text not null,"
        + TABLE_ROW_TAG2
        + " text not null,"
        + TABLE_ROW_TAG3
        + " text not null" + ")";

db.execSQL(newTableQueryString);
```

```
// Create a separate table for tags
newTableQueryString = "create table "
    + TABLE_TAGS + " ("
    + TABLE ROW ID
```