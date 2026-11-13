```
return true;
} });
```

```
listNote.setOnItemClickListener(new
AdapterView.OnItemClickListener() {
...
```

Now you can long click to delete a note. In an app that we are going to release to real users, we might like to use a popup dialog window to confirm their decision before deleting the note.