```
else
    folder.FullPathToCoverImage = "default.jpg";
if(account != null)
    folder.Username = account.Username;
}
result = folders.ToList();
}
return result;
}
```

The next interesting method in this repository is the GetFriendsFolders. This method introduces a new LINQ concept – Union. The Union takes all the items from one collection and merges it with another collection of items. The items that are the same in both the lists are merged so that the resulting list of items is unique.

```
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