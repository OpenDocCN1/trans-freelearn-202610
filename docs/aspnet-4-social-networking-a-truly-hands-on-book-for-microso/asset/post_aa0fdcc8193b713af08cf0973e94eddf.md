
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

As you can see, this method accepts a **Dictionary** collection, that contains a list of **FileIDs** and fileDescriptions. We then open up the DataContext. Then from the Dictionary collection we get a list of unique **FileIDs**. We then make one trip to the database to get all of the File objects we need using the **Contains()** method of the **FileID** collection. With the collection of appropriate files in hand we can then iterate through each of them setting the description to the value passed in via the **Dictionary** collection. Once all this leg work is done, we can call the SaveChanges method. Keep in mind that we have one working DataContext for this entire operation. It is why this method of updation works!

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

This method starts off first by attempting to identify the **FileType** that we are dealing with, which gets us the root folder for that type of file.

Keep in mind that if you want to store files at multiple locations and use the **FileSystemFolders**, then this will need to be tweaked a bit. The concept shown here is for a single root file folder!

> Another point worth noting is that the file deletion may run into issues if the file is locked. If you run into issues, you can possibly create an entry into a separate table and have a job running that regularly scans the table and deletes the files.

With the root path identified we then create the **fullPath**. This variable is created by making a call into our WebContext class, that determines the root path on the server. We then add **Files**, which is the files' root folder. Then comes the path that we just configured. The next portion may seem odd at the moment. When we upload files (coming later) we upload to a directory with the name of the folder's creation date, year, and month.

With the **fullPath** configured appropriately we then check to make sure that the directory actually exists. Now we can step through each file that we created. We check to see if the file exists at the specified location and then call System.IO.File.Delete. We do this for each type of file we create in our upload process. Keep in mind that the upload and delete processes are fairly tied at the hip. So if we change something on one end we need to make the changes at the other end too!

## Media Galleries

With the files deleted we take the file system beautification one step further. We need to check to see if the folder is now empty. If it is, then we delete the folder too!

### FolderRepository

There are a couple of methods in the FolderRepository that are worth covering.

- GetFoldersByAccountID
- GetFriendsFolders

The GetFoldersByAccountID does just what it says! It takes in an AccountID and performs a search to get all the folders by the passed in AccountID. It does go a bit beyond that. It then iterates through each folder in the resulting list and generates the file system path for the folder's cover image. This determination is then assigned to the FullPathToCoverImage. If there is no cover image found then the default image is assigned. Once we have this taken care of, we return the list of folders.

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

The next interesting method in this repository is the GetFriendsFolders. This method introduces a new LINQ concept – Union. The Union takes all the items from one collection and merges it with another collection of items. The items that are the same in both the lists are merged so that the resulting list of items is unique.

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

In our method we take in a list of our Friends. We then iterate over this collection and with each pass we check to see if we have less than 50 folders (we chose a random number of items to show on our album homepage). If we have less than 50, then we get all the folders for the current friend and merge it into our result list using the Union method. We continue to do this until we are either out of Friends or have 50 folders to show on the homepage.

As stated earlier we will be building a Flash-based file upload client. This client will be responsible for selecting a group of files on the local file system. It will then pass the files one at a time to our server side receiving page. When we discuss photo files (JPG, GIF, and so on) we will also look at some basic image processing concepts. We will also provide a Silverlight upload control as an alternate option and will reuse the control already available on codeplex (http://silverlightfileupld.codeplex.com/) for this. There are other options like AJAX-based controls as well, but we wanted to bring in a bit of Rich Internet Application (RIA) flavor and hence the use of Flash and Silverlight.

### Multiple file upload

Our file receiver will be able to receive multiple files at any one time. In our case we will be primarily concerned with receiving image files. But we will set it up to receive other files as well. The receiver in our case (ReceiveFiles.aspx.cs) will be housed inside a webpage in our Files directory. We will explain the code here, but will only show the relevant parts here and not the entire code.

The beginning of our receiver will be responsible for spinning up some variables and objects. The only interesting thing to note here is the sizesToMake collection. This will hold all the sizes, which we will generate further down the line. If we want to add an additional size or modify one of our existing sizes we would do that here.

```csharp
Dictionary<string,int> sizesToMake = new Dictionary<string,int>();
```

Then in the Page_Load() method we have the initialization of our objects and our sizes collection. We then interrogate a query string value to see what sort of file we are dealing with. In our case we will be working with photos, but we might deal with something else later.

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
