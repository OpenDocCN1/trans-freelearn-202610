```
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
```

As you can see, this method accepts a **Dictionary** collection, that contains a list of **FileIDs** and fileDescriptions. We then open up the DataContext. Then from the Dictionary collection we get a list of unique **FileIDs**. We then make one trip to the database to get all of the File objects we need using the **Contains()** method of the **FileID** collection. With the collection of appropriate files in hand we can then iterate through each of them setting the description to the value passed in via the **Dictionary** collection. Once all this leg work is done, we can call the SaveChanges method. Keep in mind that we have one working DataContext for this entire operation. It is why this method of updation works!

```
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
        var listOffiles = dc.Files.Where(f => f.DefaultFolderID == folder.FolderID).ToList();
        foreach (File f in ListOffiles)
        {
            dc.Files.DeleteObject(f);
        }
        dc.SaveChanges();
    }
}
```
```