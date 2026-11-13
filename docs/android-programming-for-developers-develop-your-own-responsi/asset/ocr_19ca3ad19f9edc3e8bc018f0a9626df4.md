# # Deleting a note – introducing OnLongClick

As a final improvement to Note To Self and as an excuse to talk about a neat Android UI feature, we will add the ability to delete a note. Context actions are often chosen in Android apps by long clicking. Long clicking is when the user holds their finger on the device screen rather than simply tapping and removing their finger.

First, let's add a delete note method to the NoteAdapter class. Here is the code that removes a Note object from the array list and then asks the adapter to update itself and ListView:

```
public void deleteNote(int n){
    noteList.remvoe(n);
    notifyDataSetChanged();
}
```

Now, in onCreate, we can prepare ListView to accept long clicks and then use an anonymous class to listen and respond to them. This inner class simply calls our new deleteNote method. Add this code just before the anonymous class that handles the regular click of ListView and just after the call to setAdapter. Here, I have included the context and highlighted the new code that is to be added:

```
...
listNote.setAdapter(mNoteAdapter);
// So we can long click it
listNote.setLongClickable(true);
// Now to detect long clicks and delete the note
listNote.setOnItemLongClickListener(new
AdapterView.OnItemLongClickListener() {
public boolean onItemLongClick(AdapterView<?> adapter, View view,
int whichItem, long id) {
// Ask NoteAdapter to delete this entry
mNoteAdapter.deleteNote(whichItem);
```