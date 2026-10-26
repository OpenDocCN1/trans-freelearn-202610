

```
public Task SaveAsync()
{
    string text = this.Title + "\n" + this.Text;
    return FileHelper.WriteTextAsync(this.Filename, text);
}

public async Task LoadAsync()
{
    string text = await FileHelper.ReadTextAsync(this.Filename);

    // Break string into Title and Text.
    int index = text.IndexOf('\n');
    this.Title = text.Substring(0, index);
    this.Text = text.Substring(index + 1);
}

...
}
}
```

这意味着 `NotePage` 的新版本必须在其构造函数中传入一个 `Note` 对象，而不是文件名：

```
namespace NoteTaker8
{
    class NotePage : ContentPage
    {
        Note note;

        public NotePage(Note note)
        {
            this.note = note;
            Title = "New Note";

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
            this.BindingContext = note;
```

```
entry.SetBinding(Entry.TextProperty, "Title");
editor.SetBinding(Editor.TextProperty, "Text");

// Assemble page.
this.Content = new StackLayout
{
    Children =
    {
        new Label { Text = "Title:" },
        entry,
        new Label { Text = "Note:" },
        editor,
    };
};
...
}
```

构造函数将自身的 BindingContext 设置为该 Note 对象。

## 枚举文件

NoteTaker7 的前一版本在用户从 NotePage 导航离开并返回到 HomePage 时，会将新笔记保存在本地应用程序存储中。但 NoteTaker8 的重大变化是提供了一个显示所有已保存笔记的功能。可以考虑几种策略来实现此功能。可以将本地存储中包含所有单个笔记文件名的主列表保存在一个特殊文件中。或者，程序也可以使用文件 I/O 设施来枚举文件夹中的所有笔记文件。

NoteTaker8 采用了第二种策略，但这要求文件助手增强以包含获取应用程序存储文件夹中所有文件的方法。以下是带有新方法 `GetFilesAsync` 的新 `IFileHelper`：

```
namespace PlatformHelpers
{
    public interface IFileHelper
    {
        Task<bool> ExistsAsync(string filename);

        Task WriteTextAsync(string filename, string text);

        Task<string> ReadTextAsync(string filename);

        Task<IEnumerable<string>> GetFilesAsync();

        Task DeleteFileAsync(string filename);
    }
}
```

请注意，还添加了 `DeleteFileAsync` 方法。目前还不需要，但在应用程序的下一个版本中将需要它。

还请注意，命名空间已更改为更通用的名称。你使用 `DependencyService` 创建的接口和类很可能在其他应用程序中也很有价值。为所有 `DependencyService` 助手使用一致的命名空间名称会很方便。

新 `IFileHelper` 的 iOS 和 Android 实现类似。以下是 iOS 版本，但仅显示新方法：

```
namespace PlatformHelpers.iOS
{
    class FileHelper : IFileHelper
    {
        ...

        public Task<IEnumerable<string>> GetFilesAsync()
        {
            // Sort the filenames.
            IEnumerable<string> filenames =
                from filepath in Directory.EnumerateFiles(GetDocsFolder())
                select Path.GetFileName(filepath);

            return Task<IEnumerable<string>>.FromResult(filenames);
        }

        public Task DeleteFileAsync(string filename)
        {
            File.Delete(GetFilePath(filename));
            return Task.FromResult(true);
        }

        string GetDocsFolder()
        {
            return Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
        }

        string GetFilePath(string filename)
        {
            return Path.Combine(GetDocsFolder(), filename);
        }
    }
}
```

`Directory.EnumerateFiles` 方法返回完全限定的文件名，因此 `GetFilesAsync` 使用少量 LINQ 去掉目录路径，只保留文件名。这是为了与 `WriteTextAsync` 和 `ReadTextAsync` 方法兼容，这些方法只需要文件名并自行提供目录路径。

Windows Phone 版本使用 WinRT `StorageFolder` 方法：

```
namespace PlatformHelpers.WinPhone
{
    ...

    public async Task<IEnumerable<string>> GetFilesAsync()
    {
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;

        IEnumerable<string> filenames =
            from storageFile in await localFolder.GetFilesAsync()
            select storageFile.Name;

        return filenames;
    }

    public async Task DeleteFileAsync(string filename)
    {
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await localFolder.GetFileAsync(filename);
        await storageFile.DeleteAsync();
    }
}
```

最后，可移植类库中的静态 `FileHelper` 类也已扩展：

```
namespace PlatformHelpers
{
    static class FileHelper
    {
        static IFileHelper fileHelper = DependencyService.Get<IFileHelper>();

        public static Task<bool> ExistsAsync(string filename)
        {
            return fileHelper.ExistsAsync(filename);
        }

        public static Task WriteTextAsync(string filename, string text)
        {
            return fileHelper.WriteTextAsync(filename, text);
        }

        public static Task<string> ReadTextAsync(string filename)
        {
            return fileHelper.ReadTextAsync(filename);
        }

        public static Task<IEnumerable<string>> GetFilesAsync()
        {
            return fileHelper.GetFilesAsync();
        }

        public static Task DeleteFileAsync(string filename)
        {
```

```
return fileHelper.DeleteFileAsync(filename);
```

这些现在是 FileHelper 类的最终版本（至少对于 NoteTaker 应用程序而言）。

## ListView

NoteTaker8 需要一种列出所有笔记的方式。它可以使用 Label 视图来显示每条笔记的标题，然后将所有这些 Label 视图收集到 ScrollView 内的 StackLayout 中。但存在一个视图可以为你完成大部分工作，甚至更多。这就是 ListView，每当你需要显示可滚动的对象列表时，这就是你的首选视图——特别是如果对象是同一种类型。

基本上你用一组对象填充 ListView，ListView 就会显示它们。你可能会认为 ListView 显示的对象必须是某种视图，但事实并非如此。ListView 显示的对象通常是数据对象，你可以精确控制 ListView 如何显示这些数据对象。关于其工作原理的完整讨论将在稍后的章节中进行，遗憾的是该章节不在本预览版中。

ListView 显示的项目是可选择的。用户只需轻按一个项目即可选择它。ItemSelected 事件在新项目被选择时发出信号，而 SelectedItem 属性则揭示该项目。在 NoteTaker 的下一个版本中，此功能将用于编辑和删除笔记。

你通过将 ListView 的 ItemsSource 属性设置为实现 IEnumerable 接口的集合对象来指定要显示的项目。这是与最通用类型的集合相关联的 .NET 接口。数组实现了 IEnumerable，因此数组符合条件。更常见的是，ListView 的 ItemsSource 属性被设置为 List<T> 的实例。例如，如果 list 是 List<T> 的实例，那么你可以这样设置 ItemsSource：

```
listView.ItemsSource = list;
```

ListView 随后会显示集合中的所有项目，如有必要则进行滚动。

非常经常地，你希望 ListView 显示的数据项集合作为另一个对象的属性公开。例如，假设一个名为 Data 的类定义了一个名为 CollectionProp 的属性，该属性实现了 IEnumerable，并且 data 是该类的一个实例。你可以这样设置 ItemsSource 属性：

```
listView.ItemsSource = data.CollectionProp;
```

但如果在 CollectionProp 属性已设置为 ItemsSource 属性之后集合发生了变化，会发生什么？

对于集合而言，“变化”这个词究竟意味着什么？

