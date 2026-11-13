```
// Attach the adapter to the ListView
setListAdapter(cursorAdapter);
}
```

Now, we can handle what happens when the user taps on a title in ListView. We implement the onListItemClick method, and all we need is the position parameter of this method in order to do our work.

We use the moveToPosition method on our Cursor and pass in position to set the cursor to the right place for the next line of code.

We then declare and initialize an int variable called dBID with the value of the _id column from the database with this line of code:

```
int dBID = mCursor.getInt(
mCursor.getColumnIndex(
DataManager.TABLE_ROW_ID));
```

Finally, we can call the onTitlesListItemSelected method of our interface to pass the appropriate _id value to MainActivity.

Add the onListItemClick method we have just discussed to TitlesFragment:

```
public void onListItemClick(ListView l, View v, int position, long id) {
    // Move the cursor to the clicked item in the list
    mCursor.moveToPosition(position);

    // What is the database _id of this item?
    int dBID = mCursor.getInt(
    mCursor.getColumnIndex(
    DataManager.TABLE_ROW_ID));

    // Use the interface to send the clicked _id
    mActivityComs.onTitlesListItemSelected(dBID);
}
```

Of course, we haven't initialized our mActivityComs member yet, and we do so the same way we did in Chapter 19, Using Multiple Fragments, and set it to null the same way also.

Add the final two methods to achieve this at the appropriate times: