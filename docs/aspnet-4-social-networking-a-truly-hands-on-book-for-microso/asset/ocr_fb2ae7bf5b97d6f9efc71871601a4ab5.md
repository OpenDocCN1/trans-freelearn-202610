# GetMemberAccountIDsByGroupID

This particular method is not complex by any means. But you will notice that it is creating a list of members from two data sources. It first looks at all the members defined by the GroupMembers table. It then takes into account that the owner of the Group is not in that list, so it also extracts the owner to add to this list.

```
```csharp
//Fisharoo/DataAccess/Repositories/GroupMemberRepository.cs
public List<int> GetMemberAccountIDsByGroupID(Int32 GroupID)
{
    List<int> result = new List<int>();
    using(FisharooDataContext dc = conn.GetContext())
    {
        result = dc.GroupMembers.Where(gm => gm.IsApproved &&
                         gm.GroupID == GroupID).
                         Select(gm => gm.AccountID).ToList();
        result.Add(dc.Groups.Where(g => g.AccountID ==
                    GroupID).Select(gm => gm.AccountID).FirstOrDefault());
    }
    return result;
}
```
```

# DeleteGroupMembers

This is a method that is performing a bulk-style action. We are selecting a list of members based on the IDs that were passed into the method. And we are then passing that list of GroupMembers into the DeleteAllOnSubmit method allowing us to perform a batch delete. This will support our multi-select page on the member management pages later.

```
```csharp
//Fisharoo/DataAccess/Repositories/GroupMemberRepository.cs
public void DeleteGroupMembers(List<int> MembersToDelete, int
                               GroupID)
{
    using(FisharooDataContext dc = conn.GetContext())
    {
        IEnumerable<GroupMember> members =
            dc.GroupMembers.Where(gm =>
                MembersToDelete.Contains(gm.AccountID)
                && gm.GroupID == GroupID);
        foreach(GroupMember grpmember in members) {
            dc.GroupMembers.DeleteObject(grpmember);
        }
        dc.SaveChanges();
    }
}
```
```