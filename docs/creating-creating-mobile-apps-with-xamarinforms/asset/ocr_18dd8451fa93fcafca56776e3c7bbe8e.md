```
```csharp
openOp.Completed = (asyncInfo2, asyncStatus2) =>
{
    {
        IRandomAccessStream stream = asyncInfo2.GetResults();
        DataReader dataReader = new DataReader(stream);
        uint length = (uint)stream.Size;
        DataReaderLoadOperation loadOp = dataReader.LoadAsync(length);
        loadOp.Completed = (asyncInfo3, asyncStatus3) =>
        {
            string text = dataReader.ReadString(length);
            dataReader.Dispose();

            // Break string into Title and Text.
            int index = text.IndexOf('\n');
            this.Title = text.Substring(0, index);
            this.Text = text.Substring(index + 1);
        };
    };
};
```

```csharp
#endif

    }
}
```
```

Notice the `Dispose` calls on the `DataWriter` and `DataReader` methods. It might be tempting to remove these calls under the assumption that the objects are disposed automatically when the objects go out of scope, but this is not the case. If `Dispose` is not called, the files remain open,

But the big question is: Why has all this code been moved to the `Note` class? Why isn’t it isolated in a separate `FileHelper` class as in the previous version of the program?

The problem is that a method that requires asynchronous callbacks to obtain an object can’t directly return that object to the caller. Look at the `Load` method here. When that `Load` method is called in a Windows Phone program, the `localFolder` variable is set, and the `createOp` object is set, but as soon as the `Completed` property is set to the asynchronous callback method, the `Load` method returns to the caller. But the method doesn’t yet have anything to return! The `GetFileAsync` operation is proceeding in the background in a secondary thread. Only later does the `Completed` callback method execute for the next step of the job. When the contents of the file are finally read within these nested callbacks, the contents must be stored somewhere. Fortunately, the code is in the `Note` class, so the results can be stored in the `Title` and `Text` properties.

In the previous version of this program, the `Exists` method returned a Boolean to indicate the existence of a file. That code needs to be moved to the `NoteTaker3SamplePage` class where it has access to the `Button` whose `IsEnabled` property must be set:

```
```csharp
using System;

#if !WINDOWS_PHONE
using System.IO;
#else
```
```