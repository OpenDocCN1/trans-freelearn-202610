```
{
    App.NoteFolder.Notes.Add(note);
}
}

// Detach handlers for Suspending and Resuming events.
helper.Suspending -= OnSuspending;
helper.Resuming -= OnResuming;

base.OnDisappearing();
}

void OnSuspending()
{
    // Save transient data and state.
    string str = note.Filename + "\x1F" +
        isNoteEdit.ToString() + "\x1F" +
        note.Title + "\x1F" +
        note.Text;

    // Run entire WriteTextAsync in separate thread.
    Task task = Task.Run(() =>
    {
        FileHelper.WriteTextAsync(App.TransientFilename, str);
    });

    // Wait for it to finish before finishing event handler.
    task.Wait();
}

async void OnResuming()
{
    // Delete transient data and state.
    if (await FileHelper.ExistsAsync(App.TransientFilename))
    {
        await FileHelper.DeleteFileAsync(App.TransientFilename);
    }
}
}
```

But take a look at how the OnSuspending handler calls FileHelper.WriteTextAsync: It defines a lambda function with that call and passes it to a to a Task.Run creation method that returns a Task object, and then it calls Wait on that object. What’s that all about?

This takes care of a problem that manifests itself in Windows Phone. When Windows Phone is suspending a program, the current PhoneApplicationService fires a Deactivated event. When the handler for that Deactivated event returns, the Windows Phone operating system assumes that the application has done everything it needs to do and will then suspend the app’s activities. Sounds reasonable, right?

However, if the Deactivated handler is calling asynchronous methods—such as are required for