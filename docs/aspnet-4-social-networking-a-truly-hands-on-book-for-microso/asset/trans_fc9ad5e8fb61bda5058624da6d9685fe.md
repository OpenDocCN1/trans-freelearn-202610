

```csharp
foreach (File file in files)
{
    file.Description = fileDescriptions.Where(f=>f.FileID==file.FileID)
    .Select(f=>f.Value).FirstOrDefault();
}
dc.SaveChanges();
}
}
```

如你所见，此方法接受一个 **Dictionary** 集合，其中包含 **FileID** 和文件描述的列表。然后我们打开 DataContext。接着，从 Dictionary 集合中获取唯一的 **FileID** 列表。然后，我们只往返数据库一次，使用 **FileID** 集合的 **Contains()** 方法获取所需的全部 File 对象。拿到相应的文件集合后，我们就可以遍历每个文件，将通过 **Dictionary** 集合传入的值设置为文件描述。等所有这些准备工作完成后，就可以调用 SaveChanges 方法。请记住，整个操作过程中我们只使用一个 DataContext。这正是这种更新方法有效的原因！

```csharp
//Fisharoo/DataAccess/Repositories/FileRepository.cs
public void DeleteFilesInFolder(string filePath, Folder folder)
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        List<File> files = GetFilesByFolderID(folder.FolderID);
        foreach (File file in files)
        {
            DeleteFileFromFileSystem(filePath, folder, file);
        }
        //Entity Framework 4 does not support any method to bulk delete, hence only way is to iterate through resultset
        //and delete one by one, another way is to use dc.ExecuteStoreQuery() and pass SQL String
        //dc.ExecuteStoreQuery() is efficient but does not use LINQ syntax
        var listOfFiles = dc.Files.Where(f => f.DefaultFolderID == folder.FolderID).ToList();
        foreach (File f in listOfFiles)
        {
            dc.Files.DeleteObject(f);
        }
        dc.SaveChanges();
    }
}
```

```csharp
if (System.IO.File.Exists(fullPath + file.FileSystemName + "__t." + file.Extension))
    System.IO.File.Delete(fullPath + file.FileSystemName + "__t." + file.Extension);
if (System.IO.File.Exists(fullPath + file.FileSystemName + "__s." + file.Extension))
    System.IO.File.Delete(fullPath + file.FileSystemName + "__s." + file.Extension);
if (System.IO.File.Exists(fullPath + file.FileSystemName + "__m." + file.Extension))
    System.IO.File.Delete(fullPath + file.FileSystemName + "__m." + file.Extension);
if (System.IO.File.Exists(fullPath + file.FileSystemName + "__l." + file.Extension))
    System.IO.File.Delete(fullPath + file.FileSystemName + "__l." + file.Extension);

if (Directory.GetFiles(fullPath).Count() == 0)
    Directory.Delete(fullPath);
}
}
```

此方法首先尝试识别我们要处理的 **FileType**，由此获得该类型文件的根文件夹。

请记住，如果你想将文件存储到多个位置并使用 **FileSystemFolders**，则需要对此稍作修改。这里展示的概念适用于单一根文件文件夹！

> 另一点值得注意的是，如果文件被锁定，文件删除可能会遇到问题。如果遇到问题，你可以在单独的表中创建一个条目，并运行一个定期扫描该表并删除文件的作业。

确定了根路径后，我们接着创建 **fullPath**。这个变量是通过调用我们的 WebContext 类创建的，该类负责确定服务器上的根路径。然后我们添加 **Files**，即文件的根文件夹。接下来就是刚刚配置的路径。下一部分目前可能看起来有些奇怪。当我们上传文件（稍后会讲到）时，我们会上传到一个以文件夹创建日期、年份和月份命名的目录中。

正确配置 **fullPath** 后，我们接着检查目录确实存在。现在，我们可以逐个遍历我们创建的每个文件。我们检查文件在指定位置是否存在，然后调用 System.IO.File.Delete。我们对上传过程中创建的每种文件类型都这样做。请记住，上传和删除过程是紧密相连的。因此，如果我们在某一端做了更改，也需要在另一端做相应的更改！

## 媒体库

删除文件后，我们进一步对文件系统进行美化。我们需要检查文件夹现在是否为空。如果为空，则也删除该文件夹！

### FolderRepository

在 FolderRepository 中有几个值得介绍的方法。

- GetFoldersByAccountID
- GetFriendsFolders

GetFoldersByAccountID 名副其实！它接收一个 AccountID，并执行搜索以获取传入 AccountID 的所有文件夹。它还不止于此。然后，它遍历结果列表中的每个文件夹，并为该文件夹的封面图片生成文件系统路径。这个结果随后被赋给 FullPathToCoverImage。如果没有找到封面图片，则分配默认图片。处理完这一切后，我们返回文件夹列表。

```csharp
//Fisharoo/DataAccess/Repositories/FolderRepository.cs
public List<Folder> GetFoldersByAccountID(Int32 AccountID)
{
    List<Folder> result = new List<Folder>();
    using (FisharooDataContext dc = conn.GetContext())
    {
        var account = dc.Accounts.Where(a => a.AccountID == AccountID).FirstOrDefault();

        IEnumerable<Folder> folders = (from f in dc.Folders
                                       where f.AccountID == AccountID
                                       orderby f.CreateDate descending
                                       select f);
        foreach (Folder folder in folders)
        {
            var fullPath = (from f in dc.Files.Include("FileType")
                            join ft in dc.FileTypes on f.FileTypeID equals ft.FileTypeID
                            where f.DefaultFolderID == folder.FolderID
                            select f).FirstOrDefault();
            if (fullPath != null)
            {
                String FullPathToCoverImage = fullPath.CreateDate.Year.ToString() + "/" + fullPath.CreateDate.Month.ToString() + "/" + fullPath.FileSystemName.ToString() + "_S_" + fullPath.FileType.Name;
                folder.FullPathToCoverImage = FullPathToCoverImage;
            }
            else
                folder.FullPathToCoverImage = "default.jpg";
            if (account != null)
                folder.Username = account.Username;
        }
        result = folders.ToList();
    }
    return result;
}
```

这个仓库中另一个有趣的方法是 GetFriendsFolders。此方法引入了一个新的 LINQ 概念——Union。Union 会获取一个集合中的所有项，并将其与另一个集合中的项合并。两个列表中相同的项会被合并，从而最终得到的列表中的项是唯一的。

```csharp
//Fisharoo/DataAccess/Repositories/FolderRepository.cs
public List<Folder> GetFriendsFolders(List<Friend> Friends)
{
    List<Folder> result = new List<Folder>();
    foreach (Friend friend in Friends)
    {
        if (result.Count < 50)
        {
            List<Folder> folders =
                GetFoldersByAccountID(friend.MyFriendsAccountID);
            IEnumerable<Folder> result2 = result.
                Union(folders);
            result = result2.ToList();
        }
        else
            break;
    }
    return result;
}
```

在我们的方法中，我们接收一个 Friends 列表。然后遍历此集合，每次循环时检查文件夹数量是否少于 50 个（我们选择了一个随机数量来在相册首页上显示）。如果少于 50 个，则获取当前好友的所有文件夹，并使用 Union 方法将其合并到结果列表中。我们持续这样做，直到 Friends 用完或首页上已有 50 个文件夹。

如前所述，我们将构建一个基于 Flash 的文件上传客户端。该客户端负责在本地文件系统中选择一组文件，然后将文件逐个传递到服务器端的接收页面。当我们讨论照片文件（JPG、GIF 等）时，我们还会涉及一些基本的图像处理概念。我们还将提供一个 Silverlight 上传控件作为备选方案，并重用 codeplex 上已有的控件（http://silverlightfileupld.codeplex.com/）。还有其他选项，如基于 AJAX 的控件，但我们希望引入一些富互联网应用（RIA）的特色，因此使用了 Flash 和 Silverlight。

### 多文件上传

我们的文件接收器能够一次接收多个文件。就我们而言，主要关注的是接收图像文件，但我们也会将其设置为接收其他文件。在我们的场景中，接收器（ReceiveFiles.aspx.cs）将位于 Files 目录中的某个网页内。我们将在此处解释代码，但仅展示相关部分，而不是完整代码。

接收器的开头部分负责初始化一些变量和对象。这里唯一值得关注的是 sizesToMake 集合。该集合将保存所有尺寸，我们在后面会生成这些尺寸。如果我们要添加额外的尺寸或修改现有尺寸，就可以在这里进行。

```csharp
Dictionary<string,int> sizesToMake = new Dictionary<string,int>();
```

然后，在 Page_Load() 方法中，我们初始化对象和尺寸集合。接着，我们检查一个查询字符串值，以确定我们要处理的是哪种文件。在我们的场景中，我们会处理照片，但以后可能会处理其他内容。

```csharp
//determine save-to folder
switch (_webContext.FileTypeID)
{
    case 1:
        saveToFolder = "Photos/";
        break;
    case 2:
        saveToFolder = "Videos/";
        break;
    case 3:
        saveToFolder = "Audios/";
        break;
}
```

