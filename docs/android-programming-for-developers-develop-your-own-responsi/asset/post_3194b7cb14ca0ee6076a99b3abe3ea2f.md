
# Note To Self continued

Open up the Note To Self app. As a reminder, if you want to see the completed code and working app after completing this chapter, it can be found in the chapter 14/Note to self folder. As the actions in this chapter require you to jump around between different files, classes, and methods, I encourage you to follow along with the files from the download bundle opened for reference.

## Updating onCreate and preparing the UI

Now, let's prepare our UI. To do so, follow these steps:

- 1. In the layout_main.xml file, delete the temporary button with a button ID that we added previously for testing purposes.

- 2. In the onCreate method of MainActivity.java, we can delete the anonymous class that handles its clicks. In order to do so, delete the code shown next:

```java
// Temporary code
Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
    DialogShowNote dialog = new DialogShowNote();
    dialog.sendNoteSelected(mTempNote);
    dialog.show(getFragmentManager(), "");
  }
});
```

- 3. Now, switch back to layout_main.xml and drag **ListView** from the palette onto the layout.

- 4. Set its id property to listView.

## Creating a list item for ListView

Next, let's create a list item to be used within ListView. Follow the given steps for the same:

- 1. Right-click on the `layout` folder and navigate to **New | Layout resource file**. Enter `listitem` in the **Name** field and make the **Root element** field RelativeLayout.

- 2. Take a look at the next screenshot to see what we are trying to achieve:

![](img/35d01831c19688a44e50bce7c489007a_477_0.png)

- 3. Drag three **ImageView** widgets onto the top-left corner of the layout.

- 4. It will make the rest of the layout easier to complete if we enter the src properties for each of these ImageView widgets. So, we might as well add id properties while we are doing so. Add the following properties from the table:

| Widget type | Property | Value to set to |
|---|---|---|
| ImageView (left) | id | imageViewImportant |
| ImageView (left) | src | @drawable/ic_warning_black_24dp |
| ImageView (center) | id | imageViewTodo |
| ImageView (center) | src | @drawable/ic_check_box_outline_blank_black_24dp |
| ImageView (right) | id | imageViewIdea |
| ImageView (right) | src | @drawable/ic_wb_incandescent_black_24dp |

- 5. Drag a **LargeText** widget immediately below **ImageViews**, again to the immediate left-hand side of the layout.

- 6. Now, drag a **Plain TextView** widget immediately below the LargeText widget from the previous step.

- 7. Follow this table to assign values to the properties:

| Widget type | Property | Value to set to |
|---|---|---|
| LargeText | id | txtTitle |
| Plain TextView | id | txtDescription |

Now we have ListView and a layout that we can use for each list item. We can go ahead and code our BaseAdapter implementation.

Let's call our new class NoteAdapter. Here is the entire implementation with just the skeleton code for the getView method because it warrants extra discussion and presentation on its own. Add this code within the MainActivity class just before its closing curly brace } because it will be an inner class:

```java
public class NoteAdapter extends BaseAdapter {

    List<Note> noteList = new ArrayList<Note>();

    @Override
    public int getCount() {
        return noteList.size();
    }

    @Override
    public Note getItem(int whichItem) {
        return noteList.get(whichItem);
    }

    @Override
    public long getItemId(int whichItem) {
        return whichItem;
    }

    @Override
    public View getView(int whichItem, View view, ViewGroup viewGroup) {

        // Implement this method next
        return view;
    }
}
```

The preceding class is nearly identical to the code we discussed previously, except we've called it NoteAdapter. This class holds ArrayList called noteList and the getItem method returns a Note object.

Now, we will look at the code for the getView method. The first thing to note is the parameters that provide us with some useful variables. Most notably, view of the View type and whichItem of the int type.

The view object reference is, in fact, an instance of the list item that is necessary to be displayed as evaluated by BaseAdapter, and whichItem is the position in ArrayList of the Note object that needs to be displayed in it. It seems like BaseAdapter must have read our minds.

All we need to do is write the code to put the data from the Note object into our list item from the listitem.xml layout.

First, we check if (view == null). If it is true, this means that this view has not been inflated to make it ready for use. So, inside the if block we inflate view, which makes all the widgets from listitem.xml available for use in the usual way.

We can then hide any ImageView widgets depending upon the combination of whether the current Note object is TODO, Important, or Idea.

Then, we finally use the setText method on our TextView widgets to display the title and description.

Add the code for the getView method. The entire method including the signature and return statement is shown again for context with the new code highlighted as follows:

```java
@Override
public View getView(
int whichItem, View view, ViewGroup viewGroup) {

// Implement this method next
// Has view been inflated already
if(view == null){
    // If not, do so here
    // First create a LayoutInflater
    LayoutInflater inflater = (LayoutInflater)
    getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    // Now instantiate view using inflater.inflate
    // using the listitem layout
    view = inflater.inflate(R.layout.listitem, viewGroup,false);
    // The false parameter is necessary
    // because of the way that we want to use listitem
}// End if

// Grab a reference to all our TextView and ImageView widgets
TextView txtTitle = (TextView) view.findViewById(R.id.txtTitle);
TextView txtDescription = (TextView) view.findViewById
(R.id.txtDescription);
ImageView ivImportant = (ImageView) view.findViewById
(R.id.imageViewImportant);
ImageView ivTodo = (ImageView) view.findViewById(R.id.imageViewTodo);
ImageView ivIdea = (ImageView) view.findViewById
(R.id.imageViewIdea);

// Hide any ImageView widgets that are not relevant
Note tempNote = noteList.get(whichItem);
if (!tempNote.isImportant()){
    ivImportant.setVisibility(View.GONE);
}
if (!tempNote.isTodo()){
    ivTodo.setVisibility(View.GONE);
}
if (!tempNote.isIdea()){
    ivIdea.setVisibility(View.GONE);
}

// Add the text to the heading and description
txtTitle.setText(tempNote.getTitle());
txtDescription.setText(tempNote.getDescription());
return view;
}
```

Now, we will add one more method to the class. Not an overridden method, one of our own. Just before the closing curly brace } of the NoteAdapter class, add the addNote method. We will call this method when we want to add a Note object to noteList:

```java
public void addNote(Note n){
    noteList.add(n);
    notifyDataSetChanged();
}
```

The notifyDataSetChanged method does exactly what the name suggests. It tells NoteAdapter that the data in noteList has changed and that ListView might need to be updated.

Now, in the MainActivity class, modify the createNewNote method that we wrote previously. Add the call to the addNote method that we just wrote inside the NoteAdapter class. The entire method should now look like this:

```java
public void createNewNote(Note n){
    mNoteAdapter.addNote(n);
}
```

Now, when DialogNewNote calls createNewNote, createNewNote will pass it straight to the addNote method in the NoteAdapter class, and will be added to ArrayList (noteList). The adapter will be notified of the change as well, which will then trigger the BaseAdapter class to do its work and keep the view up-to-date.

Declare a new NoteAdapter object as a member so that we can access it throughout the class. Add this code just after the opening curly brace, {, of the MainActivity class:

```java
private NoteAdapter mNoteAdapter;
```

Delete this temporary member from the last chapter as well:

```java
// Temporary code
```
