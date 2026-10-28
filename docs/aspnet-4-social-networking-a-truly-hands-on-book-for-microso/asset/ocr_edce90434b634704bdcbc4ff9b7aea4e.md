# ModerationRepository

For this repository, we have a standard save method that will take in one ModerationRepository at a time and deal with it appropriately. It essentially creates entries as our users flag content. This is great for one-at-a-time record creation for our community users.

# GetModerationsGlobal()

Once we have our Moderation records created, we need to be able to see the issues so that we can accept some content and deny the others. To do this, we have a method that gets all the moderations from a global point of view GetModerationsGlobal().

```
///Fisharoo/DataAccess/Repositories/ModerationRepository.cs
public List<Moderation> GetModerationsGlobal()
{
    List<Moderation> result = new List<Moderation>();
    using(FisharooDataContext dc = conn.GetContext())
    {
        var groups = (from m in dc.Moderations
                      where m.IsDenied == null || m.IsApproved == null
                      group m by m.SystemObjectRecordID
                      into g
                      select new { g, NumberOfReports = g.Count() }
                      ).OrderByDescending
                      (g1 => g1.NumberOfReports);

        foreach (var v in groups)
        {
            result.Add(v.g.ToList()[0]);
        }
    }
    return result;
}
```

This method allows us to get all the moderations, grouped by the SystemObjectRecordID as many people could report the same bit of content. This reduced view is what we will see in our admin screens so that we can accept and reject content in a more streamlined fashion. There is no need to view every complaint!