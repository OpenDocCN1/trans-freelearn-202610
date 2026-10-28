# Building repositories

With the addition of new tables will come the addition of new repositories so that we can get the data stored in those tables. We will be creating the following repositories to support our needs:

+   - RatingRepository
+   - SystemObjectRatingOptionRepository
+   - TagRepository
+   - SystemObjectTagRepository
+   - CommentRepository

Each of our repositories will generally have a method for select by ID, select all by parent ID, save, and delete. Once you have seen one repository you have pretty much seen them all. Review previous chapters, the appendices, or the included code, for example, of a standard repository. Here we will only discuss anything that varies from standard!

# RatingRepository

Let's take a look at the RatingRepository.

# HasRatedBefore()

The HasRatedBefore() method takes in a SystemObjectID, SystemObjectRecordID, and AccountID to perform a check against the account to determine if that account has already provided a rating for the specified object. If it has, then the HasRatedBefore() method returns true, otherwise false.

```
//Fisharoo/DataAccess/Repositories/RatingRepository.cs
public bool HasRatedBefore(int SystemObjectID, long SystemObjectRecordID, int AccountID)
{
    bool result = false;
    using(FisharooDataContext dc = conn.GetContext())
    {
        if (dc.Ratings.Where(r => r.SystemObjectID == SystemObjectID && r.SystemObjectRecordID == SystemObjectRecordID && r.CreatedByAccountID == AccountID).Count() > 0)
            result = true;
    }
    return result;
}
```