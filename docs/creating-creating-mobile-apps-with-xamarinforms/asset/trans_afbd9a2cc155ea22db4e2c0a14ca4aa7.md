
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Streams;
using Xamarin.Forms;

[assembly: Dependency(typeof(NoteTaker6.WinPhone.FileHelper))]

namespace NoteTaker6.WinPhone
{
    class FileHelper : IFileHelper
    {
        public async Task<bool> ExistsAsync(string filename)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;

            try
            {
                await localFolder.GetFileAsync(filename);
            }
            catch
            {
                return false;
            }
            return true;
        }

        public async Task WriteTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            IStorageFile storageFile =
                await localFolder.CreateFileAsync(filename,
                    CreationCollisionOption.ReplaceExisting);

            using (IRandomAccessStream stream =
                await storageFile.OpenAsync(FileAccessMode.ReadWrite))
            {
                using (DataWriter dataWriter = new DataWriter(stream))
                {
                    dataWriter.WriteString(text);
                    await dataWriter.StoreAsync();
                }
            }
        }

        public async Task<string> ReadTextAsync(string filename)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            IStorageFile storageFile = await localFolder.GetFileAsync(filename);
            using (IRandomAccessStream stream = await storageFile.OpenReadAsync())
            {
                using (DataReader dataReader = new DataReader(stream))
                {
                    uint length = (uint)stream.Size;
                    await dataReader.LoadAsync(length);
                    
using System.Runtime.CompilerServices;
using System.Threading.Tasks;

namespace NoteTaker6
{
    class Note : INotifyPropertyChanged
    {
        ...

        public Task SaveAsync(string filename)
        {
            string text = this.Title + "\n" + this.Text;
            return FileHelper.WriteTextAsync(filename, text);
        }

        public async Task LoadAsync(string filename)
        {
            string text = await FileHelper.ReadTextAsync(filename);

            // Break string into Title and Text.
            int index = text.IndexOf('\n');
            this.Title = text.Substring(0, index);
            this.Text = text.Substring(index + 1);
        }

        ...
    }
}

这两个方法都不返回值，但由于它们是异步的，方法会返回一个可等待的 Task 对象。处理此类方法有几种方式。SaveAsync 方法直接返回 FileHelper.WriteTextAsync 的返回值，这也是一个 Task 对象。LoadAsync 方法没有返回语句，尽管它当然也可以以一个空的返回语句结尾。SaveAsync 方法也可以有一个隐式的空返回语句，但 WriteTextAsync 调用之前必须加上 await，并且由于存在该 await 运算符，方法需要 async 修饰符：

```
public async Task SaveAsync(string filename)
{
    string text = this.Title + "\n" + this.Text;
    await FileHelper.WriteTextAsync(filename, text);
}
```

这些新的函数定义也需要更改 NoteTaker6Page 构造函数中的代码，并且你有几种选择。你可以像这样为**加载**按钮定义 Clicked 处理程序：

```
loadButton.Clicked += (sender, args) =>
{
    note.LoadAsync(FILENAME);
};
```

你会收到编译器警告，提示你可能需要考虑使用 `await` 运算符。但严格来说，你并不需要。实际情况是，`Clicked` 处理程序调用了 `LoadAsync`，但不会等待它完成。`Clicked` 处理程序会在文件被加载之前就返回到触发事件的按钮。

你可以在 lambda 函数中使用 `await`，但必须在参数列表前加上 `async` 修饰符：

```
loadButton.Clicked += async (sender, args) =>
{
    await note.LoadAsync(FILENAME);
};
```

对于**保存**按钮，你可能希望在保存操作完成之前不要启用**加载**按钮，因此你需要在那里加上 `await`：

```
saveButton.Clicked += async (sender, args) =>
{
    await note.SaveAsync(FILENAME);
    loadButton.IsEnabled = true;
};
```

实际上，如果你开始考虑异步文件 I/O，你可能会开始感到不安——而且这是有道理的。例如，如果你按下**保存**按钮，在文件仍在保存的过程中，你又按下了**加载**按钮，会怎样？会不会导致异常？会不会因为文件的一半尚未保存而只加载了一半？

在用户界面层面是可以避免这类问题的。如果你想在特定时间禁止按下某个按钮，可以禁用该按钮。下面介绍程序如何防止按钮相互干扰或自我干扰：

```
saveButton.Clicked += async (sender, args) =>
{
    saveButton.IsEnabled = false;
    loadButton.IsEnabled = false;
    await note.SaveAsync(FILENAME);
    saveButton.IsEnabled = true;
    loadButton.IsEnabled = true;
};
```

```
loadButton.Clicked += async (sender, args) =>
{
    saveButton.IsEnabled = false;
    loadButton.IsEnabled = false;
    await note.LoadAsync(FILENAME);
    saveButton.IsEnabled = true;
    loadButton.IsEnabled = true;
};
```

对于这些非常小的文件和固态存储器，你不太可能遇到问题，但小心谨慎总没有坏处。

// Create Entry and Editor views.
Entry entry = new Entry
{
    Placeholder = "Title (optional)"
};

Editor editor = new Editor
{
    Keyboard = Keyboard.Create(KeyboardFlags.All),
    BackgroundColor = Device.OnPlatform(Color.Default,
                                    Color.Default,
                                    Color.White),
    VerticalOptions = LayoutOptions.FillAndExpand
};

// Set data bindings.
Note note = new Note();
this.BindingContext = note;
entry.SetBinding(Entry.TextProperty, "Title");
editor.SetBinding(Editor.TextProperty, "Text");

// Create Save and Load buttons.
Button saveButton = new Button
{
    Text = "Save",
    HorizontalOptions = LayoutOptions.CenterAndExpand
};

Button loadButton = new Button
{
    Text = "Load",
    IsEnabled = false,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};

// Set Clicked handlers.
saveButton.Clicked += async (sender, args) =>
{
    saveButton.IsEnabled = false;
    loadButton.IsEnabled = false;
    await note.SaveAsync(FILENAME);
    saveButton.IsEnabled = true;
    loadButton.IsEnabled = true;
};

loadButton.Clicked += async (sender, args) =>
{
    saveButton.IsEnabled = false;
    loadButton.IsEnabled = false;
    await note.LoadAsync(FILENAME);
    saveButton.IsEnabled = true;
    loadButton.IsEnabled = true;
};

# 第 4 章

# 构建应用：架构

**NoteTaker** 应用已经从概念发展为单页程序，其中包含了保存和检索笔记所需的大部分基础架构。现在它必须扩展为一个可以显示和维护多条笔记的多页应用。

本章说明了为该增强版本实现所有必要功能的步骤。需要两个重要的 Xamarin.Forms 可视化元素：第一个是 `NavigationPage`，它允许程序在多个页面之间导航；第二个是 `ListView`，它显示可滚动且可选择的项目列表，通常是相同类型的项目。在本程序中，`ListView` 将显示多个 `Note` 对象。

指定某些程序操作还需要 `ToolbarItem`。严格来说，`ToolbarItem` 并不是一个可视化元素，但它用于指定工具栏上的项目，该工具栏显示在页面顶部（iOS 和 Android）或底部（Windows Phone）。

从程序员的角度来看，传统桌面应用编程与移动应用编程之间的一大区别是需要理解应用程序生命周期。在移动应用启动到终止之间，应用可能会被挂起。此时，应用保存其数据（包括临时数据）非常重要，这些数据是在应用重新启动时重新创建应用内容所必需的。应用程序生命周期编程也是创建真实应用的重要组成部分。

# 版本 7：页面导航

到目前为止，本书中的所有程序都基于单个派生自 `ContentPage` 的类的单个实例。相比之下，**NoteTaker7** 包含两个派生自 `ContentPage` 的类，分别名为 `HomePage` 和 `NotePage`。程序启动时，会显示 `HomePage`，其中列出你在之前的会话中输入的所有笔记。用户可以从 `HomePage` 导航到 `NotePage` 以输入新笔记，或编辑、删除现有笔记，然后导航回 `HomePage`，后者会显示更新后的列表。

Xamarin.Forms 为页面导航提供的方法都由 `Xamarin.Forms` 命名空间中的 `INavigation` 接口定义：

```
public interface INavigation
{
    Task PushAsync(Page page);
    Task<Page> PopAsync();
    Task PopToRootAsync();
    Task PushModalAsync(Page page);
}
```
