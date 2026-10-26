```
entry,
new Label
{
    Text = "Note:"
},
editor,
new StackLayout
{
    Orientation = StackOrientation.Horizontal,
    Children =
    {
        saveButton,
        loadButton
    }
}
};
}

void OnSaveButtonClicked(object sender, EventArgs args)
{
    Note note = new Note
    {
        Title = entry.Text,
        Text = editor.Text
    };
    note.Save(FILENAME);
    loadButton.IsEnabled = true;
}

void OnLoadButtonClicked(object sender, EventArgs args)
{
    Note note = new Note();
    note.Load(FILENAME);
    entry.Text = note.Title;
    editor.Text = note.Text;
}
```

Notice that the Load button initialization calls FileHelper.Exists to determine if the file exists and disables the button if it does not. The button is then enabled the first time the file is saved.

You’ll want to convince yourself that this works—that the information is saved to the device (or phone simulator). Type something into the Entry and Editor, and then press Save to save that information. Clear out the Entry and Editor (or type in new text), and press Load to restore the information that was saved.

Here’s what’s really important: If you terminate the program or shut down the phone and then rerun the program, the saved file still exists.

Well, in two out of three cases, the saved file still exists. It works with iOS and Android, but not Windows Phone. Although the Windows Phone Save and Load buttons seem to work while the