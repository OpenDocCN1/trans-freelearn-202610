```
```csharp
{
    // Only save it if there's some text somewhere.
    if (!String.IsNullOrWhiteSpace(note.Title) ||
        !String.IsNullOrWhiteSpace(note.Text))
    {
        note.SaveAsync(filename);
    }
    base.OnDisappearing();
}
}
}
```
```

This means that the user doesn’t need to bother about explicitly saving the note. It’s done automatically while navigating back to the home page.

You might wonder: What happens if the user terminates the app without navigating back? Or leaves the app running with the `NotePage` still active and then shuts down the phone? Yes, that’s a problem. But it’s really an issue involving a subject known as *application lifecycle*, and that will be confronted before this chapter is concluded.

# Version 8. Notes in the ListView

As the **NoteTaker** program is enhanced to display multiple notes, a little refactoring is probably in order. Currently, the `Note` object and the filename associated with that `Note` object are not tightly linked. It’s possible for a program to call the `SaveAsync` method of `Note` with a different filename than used with the `LoadAsync` method.

Let’s fix that problem. In **NoteTaker8**, let’s pass a filename to the `Note` constructor, which saves it as a public read-only property named `Filename`. (You’ll eventually see the rationale behind making this property public. However, because this `Filename` property is set in the constructor and never changes, there’s no reason for the property to fire the `PropertyChanged` event.) That allows the `LoadAsync` and `SaveAsync` methods to be redefined with no arguments. Here are the pertinent parts of the new version:

```
```csharp
namespace NoteTaker8
{
    class Note : INotifyPropertyChanged
    {
        string title, text;

        public event PropertyChangedEventHandler PropertyChanged;

        public Note(string filename)
        {
            this.Filename = filename;
        }

        public string Filename { private set; get; }
```
```