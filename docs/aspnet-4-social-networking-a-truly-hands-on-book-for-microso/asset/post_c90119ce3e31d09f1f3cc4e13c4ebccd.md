
```csharp
int varCreatedByAccountID = ratings[0].CreatedByAccountID;

// get a list of items that have been rated before
List<long> previouslyRatedSystemObjectRecordIDs =
    dc.Ratings.Where(r =>
        r.CreatedByAccountID == varCreatedByAccountID)
    .Select(r => r.SystemObjectRecordID)
    .ToList();

foreach (Rating rating in ratings)
{
    if (!previouslyRatedSystemObjectRecordIDs.Contains(
        rating.SystemObjectRecordID))
        dc.Ratings.AddObject(rating);
}
dc.SaveChanges();
```

This method is not really that complex. First, it takes in a generic list of **Rating** objects. Once it has opened a connection and got the DataContext, we determine which ratings the user has previously rated (as we don't want more than one rating per user, per object!). We then iterate through all the passed in ratings. Once we have made sure that the user has not already rated the option in question, we add the rating object to the DataContext. With all the appropriate ratings attached, we save our changes.

## SystemObjectRatingOptionRepository

This particular repository has only three methods, **GetSystemObjectRatingOptionsBySystemObjectID()** (now that is a mouthful!), **SaveSystemObjectRatingOption()**, and **DeleteSystemObjectRatingOption()**. These are all very standard methods that do just what they say. Looking at the code for this repository is a good example of a standard repository!

## TagsRepository

This repository is used heavily in the building of our tag clouds. We have several methods to look at here.

### GetTagsBySystemObjectAndRecordID()

Finally, we come to the method that gets all the tags for a specified object down to the record level. This is for a specific item of a certain type of object. This method is used to display all the tags next to the item that was tagged.

```csharp
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

## SystemObjectTagRepository

There are three methods in this repository, GetSystemObjectsByTagID(), SaveSystemObjectTag(), and DeleteSystemObjectTag(). We will discuss only the GetSystemObjectsByTagID() method here.

### GetSystemObjectsByTagID()

This is a rather large method. It is used to get all the objects that a tag is related to. This method is used when we display the tag and its links by clicking on a tag link.

> This method allows for only one tag to be loaded currently. But that restriction is only on the method input side. All the internals of this method will work with more than one tag though.

```csharp
//Fisharoo/DataAccess/Repositories/SystemObjectTagRepository.cs
public List<SystemObjectTagWithObject> GetSystemObjectsByTagID(int TagID)
{
```

```csharp
switch (tag.SystemObjectID)
{
    case 1:
        result.Add(new SystemObjectTagWithObject() {
            SystemObjectTag = tag, Account = accounts.Where(a => a.AccountID == tag.SystemObjectRecordID).FirstOrDefault() });
        break;

    case 2:
        result.Add(new SystemObjectTagWithObject() {
            SystemObjectTag = tag, Profile = profiles.Where(p => p.ProfileID == tag.SystemObjectRecordID).FirstOrDefault() });
        break;

    case 3:
        result.Add(new SystemObjectTagWithObject() {
            SystemObjectTag = tag, Blog = blogs.Where(b => b.BlogID == tag.SystemObjectRecordID).FirstOrDefault()
        });
        break;

    case 4:
        result.Add(new SystemObjectTagWithObject() {
            SystemObjectTag = tag, BoardPost = posts.Where(p => p.PostID == tag.SystemObjectRecordID).FirstOrDefault()
        });
        break;

    case 5:
        //need to get the file for use in getting the folder as well
        File file = files.Where(f => f.FileID == tag.SystemObjectRecordID).FirstOrDefault();
        result.Add(new SystemObjectTagWithObject()
        {
            SystemObjectTag = tag,
            File = file,
            Folder = folders.Where(f => f.FolderID == file.DefaultFolderID).FirstOrDefault()
        });
        break;

    case 6:
        result.Add(new SystemObjectTagWithObject() {
            SystemObjectTag = tag, Group = groups.Where(g
```

- WebContext
- Extensions
- AccountService

## TagService

The TagService has a few things that are responsible for adding tags and calculating font sizes.

### AddTag()

We have the AddTag() method that takes care of adding Tags to our database. You may ask why we can't just take care of that in our TagRepository. In this case, we are doing more than just shoving a new record into the system. Here we need to see if we already have the tag or not. If we don't, then we need to create a new tag. If we do have the tag already, then we need to increment the count for that tag and we can save the tag back into the system.

In addition to adding a tag into the system, we also need to create the relationship for that tag and the object it is related to. This is a straightforward job. We simply spin up a new SystemObjectTag with the appropriate TagID. Then we can toss that into the database.

```csharp
//Fisharoo/BusinessLogic/TagService.cs
public void AddTag(string TagName, int SystemObjectID, long SystemObjectRecordID)
{
    Tag tag = _tagsRepository.GetTagByName(TagName);
    if (tag == null)
    {
        tag = new Tag();
        tag.CreateDate = DateTime.Now;
        tag.Name = TagName;
        tag.Count = 1;
    }
    else
    {
        tag.Count += 1;
    }
    tag = _tagRepository.SaveTag(tag);
    SystemObjectTag sysObjTag = new SystemObjectTag();
    sysObjTag.CreateDate = DateTime.Now;
    sysObjTag.CreatedByAccountID = _webContext.CurrentUser.AccountID;
    sysObjTag.CreatedByUsername = _webContext.CurrentUser.Username;
    sysObjTag.SystemObjectID = SystemObjectID;
    sysObjTag.SystemObjectRecordID = SystemObjectRecordID;
    sysObjTag.TagID = tag.TagID;
    _systemObjectTagRepository.SaveSystemObjectTag(sysObjTag);
}
```

### CalculateFontSize()

The CalculateFontSize() method is responsible for taking a collection of Tags and calculating the font size for each one. There is a bit to this calculation in that some tags may have a very large count while other tags may only have been applied to a couple of items. If those were the numbers we used to calculate the font size, we may end up with a tag that is larger than the page or one that is so small that you can't read it. For this reason, we need to get the numbers to fit between something more reasonable. This is the calculation that we will discuss.

Before we look at the method in its entirety, let's discuss some of the variables that we will be working with:

- **MinimumRange**: This variable will hold the smallest count in our set of tags. This will form the lower bound of our collection. It is global to the calculation.
- **MaximumRange**: This variable will hold the largest count in our set of tags. This will form the upper bound of our collection. It is global to the calculation.
- **Delta**: This variable will hold the difference between our **MinimumRange** and our **MaximumRange**. It is global to the calculation.
- **Tag.InitialValue**: This will hold a copy of the Count for each tag.
- **Tag.MinimumOffset**: This is the **InitialValue**–**MinimumRange**.
- **Tag.Ranged**: This value is the **MinimumOffset** divided by the **Delta**.
- **Tag.PreCalculatedValue**: This is the **Ranged** value multiplied by the difference of the largest font size and the smallest font size subtracted by one (largest-smallest-1).
- **Tag.FinalCalculatedValue**: This is the **PreCalculatedValue** plus one.
- **Tag.FontSize**: This is the **FinalCalculatedValue** plus the smallest font size.

The result of all of these calculations is a set of tags that grow smoothly regardless of the difference between your smallest tag count and your largest tag count and regardless of the difference between your smallest and largest font size.
