# GagRepository

There are two methods in this repository that we will discuss:

+   - **GetActiveGags()** will give us the ability to get a listing of all the Gags in the system so that we can manage who is gagged, who is about to be ungagged, and possibly a user who may need a Gag extension.
+   - **IsGagged()** will allow us to check to see if a user is currently gagged. It will allow us to restrict a gagged user from creating new content. We can also use it to make changes to the display such as hiding the new post button.

```
```csharp
//Fisharoo/DataAccess/Repositories/Gagpository.cs
public List<Gag> GetActiveGags()
{
    List<Gag> result = new List<Gag>();
    using(FisharooDataContext dc = conn.GetContext())
    {
        result = dc.Gags.Where(g => g.GagUntilDate >
            DateTime.Now).OrderBy(g => g.GagUntilDate).
            ToList();
    }
    return result;
}
public bool IsGagged(Int32 AccountID)
{
    bool result = false;
    using(FisharooDataContext dc = conn.GetContext())
    {
        if(dc.Gags.Where(g=>g.AccountID == AccountID &&
            g.GagUntilDate > DateTime.Now).
            FirstOrDefault() != null)
        {
            result = true;
        }
    }
    return true;
}
```
```

# ContentFilterRepository

The ContentFilterRepository currently has only one job. It returns a list of all the filters in the system.