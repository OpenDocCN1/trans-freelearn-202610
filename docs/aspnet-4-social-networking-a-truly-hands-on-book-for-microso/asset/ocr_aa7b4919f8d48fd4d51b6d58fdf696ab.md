# GetTagsBySystemObjectAndRecordID()

Finally, we come to the method that gets all the tags for a specified object down to the record level. This is for a specific item of a certain type of object. This method is used to display all the tags next to the item that was tagged.

```
//Fisharoo/DataAccess/Repositories/TagsRepository.cs
public List<Tag> GetTagsBySystemObjectAndRecordID(int SystemObjectID, long SystemObjectRecordID)
{
    List<Tag> results = null;
    using (FisharooDataContext dc = conn.GetContext())
    {
        results = (from t in dc.Tags
            join sot in dc.SystemObjectTags on t.TagID equals sot.TagID
            where sot.SystemObjectID == SystemObjectID &&
                sot.SystemObjectRecordID == SystemObjectRecordID
            select t).Distinct().OrderBy(t => t.CreateDate).ToList();
    }
    return results;
}
```

# SystemObjectTagRepository

There are three methods in this repository, GetSystemObjectByTagID(), SaveSystemObjectTag(), and DeleteSystemObjectTag(). We will discuss only the GetSystemObjectByTagID() method here.

# GetSystemObjectByTagID()

This is a rather large method. It is used to get all the objects that a tag is related to. This method is used when we display the tag and its links by clicking on a tag link.

> This method allows for only one tag to be loaded currently. But that restriction is only on the method input side. All the internals of this method will work with more than one tag though.

```
//Fisharoo/DataAccess/Repositories/SystemObjectTagRepository.cs
public List<SystemObjectTagWithObject> GetSystemObjectsByTagID(int TagID)
{
```