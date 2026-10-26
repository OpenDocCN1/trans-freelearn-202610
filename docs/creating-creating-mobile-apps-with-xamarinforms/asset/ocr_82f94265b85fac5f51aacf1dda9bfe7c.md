often plays a role in the relatively fast solid state file I/O on mobile devices.

Let’s improve this ReadAllText method a bit. The DataReader class implements the IDisposable interface and includes a Dispose method. Failing to call Dispose can leave the file open. Calling Dispose also closes the IRandomAccessStream on which the DataReader is based. IRandomAccessStream also implements IDisposable.

It’s a good idea to enclose IDisposable objects in using blocks. This ensures that the Dispose method is automatically called even if an exception is thrown inside the block. A better implementation of ReadAllText is this:

```
public async void ReadAllText(string filename, Action<string> completed)
{
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    IStorageFile storageFile = await localFolder.GetFileAsync(filename);
    using (IRandomAccessStream stream = await storageFile.OpenReadAsync())
    {
        using (DataReader dataReader = new DataReader(stream))
        {
            uint length = (uint)stream.Size;
            await dataReader.LoadAsync(length);
            string text = dataReader.ReadString(length);
            completed(text);
        }
    }
}
```

Now the explicit call to Dispose is not required.

Of course, we still have the annoyance of passing a callback function to the ReadAllText method. But what’s the alternative? If the method actually returns to the caller at the first await operator, how can the method return the text contents of the file? Trust the C# compiler. If it can implement await, surely it can also allow you to create your own asynchronous methods.

Let’s replace ReadAllText with a method named ReadTextAsync. The new name reflects the fact that this method is itself asynchronous and can be called with the await operator to return a string with the contents of the file.

To do this, the ReadTextAsync method needs to return a Task object. The Task class is defined in System.Threading.Tasks and is the standard .NET representation of an asynchronous operation. The Task class is quite extensive, but only a little bit of it is necessary in this context. The System.Threading.Tasks namespace actually defines two Task classes:

+   - Task for asynchronous methods that return nothing

+   - Task<TResult> for asynchronous methods that return an object of type TResult.

These are the .NET equivalences of the Windows 8 IAsyncAction and IAsyncOperation<TResult> interfaces, and there are extension methods that convert the Task objects to IAsyncAction and IAsyncOperation<TResult> objects.