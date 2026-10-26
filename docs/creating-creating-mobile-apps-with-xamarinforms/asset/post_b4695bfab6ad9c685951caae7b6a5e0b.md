
There are actually three different ways that the collection might change! Understanding the different types of change associated with collections will help you design your data and user interface so that the `ListView` always displays the latest and greatest data items.

Here’s the first type of change: Perhaps the `CollectionProp` property of the `Data` instance returns a particular `List<T>` object when the application starts running, and then something happens and the property returns a different `List<T>` object. It’s a completely different collection with different items. If that is the case, then the `Data` class should implement `INotifyPropertyChanged`, and fire a `PropertyChanged` event when the `CollectionProp` property changes. Accordingly, the `ItemsSource` property of `ListView` should not simply be set to the `CollectionProp` property but must be bound to the property with a data binding:

```
listView.BindingContext = data;
listView.SetBinding(ListView.ItemsSourceProperty, "CollectionProp");
```

That’s one way the collection might change. Here’s a second way: The `List<T>` object returned by the `CollectionProp` property might remain the same while the program is running, but items might be added to or removed from the collection dynamically, or perhaps reordered or resorted in some way, and of course you would like the `ListView` display to reflect the updated contents.

This is the type of change that **NoteTaker8** will need to handle. The `ListView` in **NoteTaker8** will display the collection of notes, but the user can add notes and delete notes to the collection while the program is running. The `ListView` must respond to those changes. At first, this sounds like a difficult requirement. How can the `ListView` determine that the collection that is set to (or bound to) its `ItemsSource` property has acquired a new item or lost an existing item?

Through an event, of course! You already know about the `INotifyPropertyChanged` interface in the `System.ComponentModel` namespace defined like so:

```
public interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler PropertyChanged;
}
```

The `System.Collections.Specialized` namespace contains a related and similar interface named `INotifyCollectionChanged`:

```
public interface INotifyCollectionChanged
{
    event NotifyCollectionChangedEventHandler CollectionChanged;
}
```

A collection class can implement this interface and notify the outside world when an object has been added to or removed from the collection, or the items have been reordered in some way. The event argument associated with `NotifyCollectionChangedEventHandler` provides details on this change.

`ListView` knows about `INotifyCollectionChanged`. Whenever a collection object is set to its `ItemsSource` property, `ListView` checks if that collection object implements this interface. If so, `ListView` attaches a handler on the `CollectionChanged` event of that collection object. The `ListView` is then notified when objects are added to or removed from the collection (or if the objects are reordered in some way), and updates the `ListView` display accordingly.

Now all we need is an actual class that implements `INotifyCollectionChanged`, and there is one: It’s in the `System.Collections.ObjectModel` namespace and it’s a generic class called `ObservableCollection`, which is a great name because it implies that by using the `CollectionChanged` event you can observe what’s going on inside the collection.

There is also a third way that a collection can change. Perhaps the collection object itself doesn’t change, and objects are not added to, or removed from, the collection, but properties of the objects in the collection change. For example, suppose your collection contains a bunch of `Weather` objects for major cities. The collection itself never changes but the `Weather` class contains a `Temperature` property that changes throughout the day. If this `Weather` object implements `INotifyPropertyChanged` and fires `PropertyChanged` events when the `Temperature` property changes, the `ListView` can respond to those changes as well.

NoteTaker8 definitely requires an `ObservableCollection`, so let’s handle that first. Let’s create a class that will store a collection of `Note` objects. The `NoteFolder` class has a single property named `Notes` that is of type `ObservableCollection<Note>`. The constructor calls an asynchronous method that obtains a list of all the files using `FileHelper.GetFilesAsync`, and then sorts them by filename. (Keep in mind that the filenames are constructed from the date and time when the `Note` object was created, so these filenames will be sorted into the order in which the files were created.) Then a `Note` object is created for each filename, the `LoadAsync` method is called to load the file into the `Note` object, and it’s added to the `Notes` collection:

```
namespace NoteTaker8
{
    class NoteFolder
    {
        public NoteFolder()
        {
            this.Notes = new ObservableCollection<Note>();
            GetFilesAsync();
        }

        public ObservableCollection<Note> Notes { private set; get; }

        async void GetFilesAsync()
        {
            // Sort the filenames.
            IEnumerable<string> filenames =
                from filename in await FileHelper.GetFilesAsync()
                where filename.EndsWith(".note")
                orderby (filename)
                select filename;

            // Store them in the Notes collection.
```

Notice that the `ItemsSource` property of the `ListView` is set to `App.NoteFolder.Notes`. The `ListView` is scrollable, so if it shares a `StackLayout` with some other view (such as `Button` on this page), it must have a `VerticalOptions` setting of `FillAndExpand`.

When the `Button` is clicked, `HomePage` constructs a unique filename, instantiates a `Note` object, and navigates to `NotePage`, which (as you’ve seen) sets its `BindingContext` to that `Note`. What you haven’t seen yet is the new `OnDisappearing` override of `NotePage`:

```
namespace NoteTaker8
{
    class NotePage : ContentPage
    {
        ...
        protected override void OnDisappearing()
        {
            // Only save it if there's some text somewhere.
            if (!String.IsNullOrWhiteSpace(note.Title) ||
                !String.IsNullOrWhiteSpace(note.Text))
            {
                note.SaveAsync();
                App.NoteFolder.Notes.Add(note);
            }
            base.OnDisappearing();
        }
    }
}
```

The new `OnDisappearing` saves the new `Note` object and adds it to the `App.NoteFolder.Notes` collection. As the `HomePage` comes back into view, the `ListView` shows the new `Note` object.

However, as you experiment with adding new `Note` objects, you’ll find that the `ListView` display isn’t all that illuminating:

```
Children =
{
    new Label { Text = "Title:" },
    entry,
    new Label { Text = "Note:" },
    editor,
};

if (isNoteEdit)
{
    // Cancel button.
    Button cancelButton = new Button
    {
        Text = "Cancel",
        HorizontalOptions = LayoutOptions.CenterAndExpand
    };

    cancelButton.Clicked += async (sender, args) =>
    {
        bool confirm = await this.DisplayAlert("Note Taker",
            "Cancel note edit?",
            "Yes", "No");

        if (confirm)
        {
            // Reload note.
            await note.LoadAsync();

            // Return to home page.
            await this.Navigation.PopAsync();
        }
    };

    // Delete button
    Button deleteButton = new Button
    {
        Text = "Delete",
        HorizontalOptions = LayoutOptions.CenterAndExpand
    };

    deleteButton.Clicked += async (sender, args) =>
    {
        bool confirm = await this.DisplayAlert("Note Taker",
            "Delete this note?",
            "Yes", "No");

        if (confirm)
        {
            // Delete Note file and remove from collection.
            await note.DeleteAsync();
            App.NoteFolder.Notes.Remove(note);

            // Return to home page.
            await this.Navigation.PopAsync();
        }
    }
```
