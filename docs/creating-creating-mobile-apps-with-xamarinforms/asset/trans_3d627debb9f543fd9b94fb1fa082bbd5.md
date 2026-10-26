

```
}
string GetFilePath(string filename)
{
    string docsPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
    return Path.Combine(docsPath, filename);
}
}
}
```

Android 版本非常相似。Windows Phone 版本包含的方法在最内层的嵌套异步回调中调用已完成的函数：

```
```csharp
using System;
using Windows.Foundation;
using Windows.Storage;
using Windows.Storage.Streams;
using Xamarin.Forms;

[assembly: Dependency(typeof(NoteTaker3Pcl.WinPhone.FileHelper))]

namespace NoteTaker3Pcl.WinPhone
{
    class FileHelper : IFileHelper
    {
        public void Exists(string filename, Action<bool> completed)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            IAsyncOperation<StorageFile> createOp = localFolder.GetFileAsync(filename);
            createOp.Completed = (asyncInfo, asyncStatus) =>
            {
                completed(asyncStatus != AsyncStatus.Error);
            };
        }

        public void WriteAllText(string filename, string text, Action completed)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            IAsyncOperation<StorageFile> createOp =
                localFolder.CreateFileAsync(filename,
                    CreationCollisionOption.ReplaceExisting);
            createOp.Completed = (asyncInfo1, asyncStatus1) =>
            {
                IStorageFile storageFile = asyncInfo1.GetResults();
                IAsyncOperation<IRandomAccessStream> openOp =
                    storageFile.OpenAsync(FileAccessMode.ReadWrite);
                openOp.Completed = (asyncInfo2, asyncStatus2) =>
                {
                    IRandomAccessStream stream = asyncInfo2.GetResults();
                    DataWriter dataWriter = new DataWriter(stream);
                    dataWriter.WriteString(text);
                    DataWriterStoreOperation storeOp = dataWriter.StoreAsync();
                    storeOp.Completed = (asyncInfo3, asyncStatus3) =>
                    {
```

```
```csharp
public void ReadAllText(string filename, Action<string> completed)
{
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    IAsyncOperation<StorageFile> createOp = localFolder.GetFileAsync(filename);
    createOp.Completed = (asyncInfo1, asyncStatus1) =>
    {
        IStorageFile storageFile = asyncInfo1.GetResults();
        IAsyncOperation<IRandomAccessStream> openOp =
            storageFile.OpenReadAsync();
        openOp.Completed = (asyncInfo2, asyncStatus2) =>
        {
            IRandomAccessStream stream = asyncInfo2.GetResults();
            DataReader dataReader = new DataReader(stream);
            uint length = (uint)stream.Size;
            DataReaderLoadOperation loadOp = dataReader.LoadAsync(length);
            loadOp.Completed = (asyncInfo3, asyncStatus3) =>
            {
                string text = dataReader.ReadString(length);
                dataReader.Dispose();
                completed(text);
            };
        };
    };
}
```
```

为了隐藏对 `DependencyService.Get` 方法的调用，**NoteTaker3Pcl** 项目中添加了另一个文件。此类让程序中的其他代码可以使用看起来正常的静态 `FileHelper` 方法：

```
```csharp
namespace NoteTaker3Pcl
{
    static class FileHelper
    {
        static IFileHelper fileHelper = DependencyService.Get<IFileHelper>();

        public static void Exists(string filename, Action<bool> completed)
        {
            fileHelper.Exists(filename, completed);
        }

        public static void WriteAllText(string filename, string text, Action completed)
        {
            fileHelper.WriteAllText(filename, text, completed);
        }
    }
}
```
```

```
```csharp
// Check if the file is available.
FileHelper.Exists(FILENAME, (exists) =>
{
    loadButton.IsEnabled = exists;
});
```
```

这能修复 Windows Phone 上 **Load** 按钮首次按下时的问题吗？不能。`OnLoadButtonClicked` 方法仍然在从文件加载文本之前就将 `Entry` 和 `Editor` 设置为 `Note` 类的属性文本。要正常工作，该代码需要在将它们传输到 `Editor` 和 `Entry` 之前知道 `Note` 属性何时被设置。或者页面类需要知道 `Note` 对象的 `Title` 和 `Text` 属性何时发生变化。

基本问题涉及存在属性在值发生变化时不通知任何人的情况。我们能对此做些什么吗？

# 版本 4. 我会在属性更改时通知你

让我们退一步。

到目前为止，程序的所有版本都包含一个派生自 `ContentPage` 的类，该类显示一个用户界面，允许用户输入和编辑两段文本。这两段文本也存储在一个名为 `Note` 的类中。此 `Note` 类存储了页面类用户界面的底层数据。

这两个类实际上是同一数据的两个方面——一个类为用户编辑数据提供呈现，另一个类处理更低级别的任务，包括在文件系统中的加载和保存数据。

理想情况下，在任何时候，这两个类都应该处理相同的数据。但事实并非如此。页面类不知道 `Note` 类中的数据何时发生变化，而 `Note` 类也不知道 `Entry` 和 `Editor` 视图中的文本何时发生变化。

保持用户界面与底层数据同步是一个常见问题，并且有标准解决方案可以修复该问题。其中最重要的一项是名为 `INotifyPropertyChanged` 的接口。它在 .NET 的 `System.ComponentModel` 命名空间中定义如下：

```
```csharp
interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler PropertyChanged;
}
```
```

整个接口仅由一个名为 `PropertyChanged` 的事件组成，但该事件为类提供了一种简单通用的方式来通知可能感兴趣的任何其他类其某个属性的值已更改。

```
{
    text = value;
    if (PropertyChanged != null)
    {
        PropertyChanged(this, new PropertyChangedEventArgs("Text"));
    }
}
get
{
    return text;
}
}
public void Save(string filename)
{
    string text = this.Title + "\n" + this.Text;
    FileHelper.WriteAllText(filename, text, () => { });
}
public void Load(string filename)
{
    FileHelper.ReadAllText(filename, (string text) =>
    {
        // Break string into Title and Text.
        int index = text.IndexOf('\n');
        this.Title = text.Substring(0, index);
        this.Text = text.Substring(index + 1);
    });
}
}
```

各种 FileHelper 类与 NoteTaker3Pcl 中的相同。

NoteTaker4Page 类将 `Note` 的实例定义为字段（如程序的上一版本），但现在构造函数还附加了 `Note` 现在定义的 PropertyChanged 事件的处理程序：

```
note.PropertyChanged += (sender, args) =>
{
    switch (args.PropertyName)
    {
        case "Title":
            entry.Text = note.Title;
            break;
        case "Text":
            entry.Text = note.Text;
            break;
    }
};
```

这当然可以是一个命名事件处理程序，并且它可以使用 if 和 else 而不是 switch 和 case 来识别正在更改的属性。然后它将属性的新值设置为 `Entry` 或 `Editor` 的 Text 属性。

看起来不错，但它在 Windows Phone 上仍然无法工作。当你点击 Load 按钮时，你会得到一个 Unauthorized Access 异常。现在问题出在哪里？

问题在于：通常情况下，异步方法的回调不会在发起操作的同一线程中执行。相反，回调在执行异步操作的后台线程中执行。

让我们逐步跟踪：当你按下 Load 按钮时，Windows Phone 的 ReadAllText 方法执行。当文本获取到后，它会在另一个执行线程中调用 completed 方法。在 `Note` 中的 Load 方法中，该 completed 方法设置 Title 和 Text 属性。新的 Title 属性导致 PropertyChanged 事件触发，而在该处理程序中，新的 Title 属性被设置为 Entry 视图的 Text 属性。

因此，Entry 视图是从用户界面线程以外的线程访问的，而这是不允许的。这就是为什么会引发异常。

幸运的是，这个问题的修复方法相当简单。`Device` 类有一个 `BeginInvokeOnMainThread` 方法，其参数类型为 `Action`。只需将你想要在 UI 线程中执行的代码封装在该 `Action` 参数的主体中。最简单的方法是将整个 switch 和 case 包装在该回调中：

```
note.PropertyChanged += (sender, args) =>
{
    Device.BeginInvokeOnMainThread(() =>
    {
        switch (args.PropertyName)
        {
            case "Title":
                entry.Text = note.Title;
                break;

            case "Text":
                editor.Text = note.Text;
                break;
        }
    });
};
```

`Device.BeginInvokeOnMainThread` 有效地等待 UI 线程从操作系统的线程调度程序获得时间片，然后运行指定的代码。

有了这个更改，你会发现重新运行 Windows Phone 应用后，你只需按一次 Load，Entry 和 Editor 就会被设置成保存的值。它们不是在 Load 按钮的处理程序中设置的，而是在属性实际使用从文件加载的值更新时，在 PropertyChanged 处理程序中设置的。

