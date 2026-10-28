
## Sending an invitation

With all three of the above methods we will end up sending out an invitation email. We could simply send out an email with a link to the site. However, we need to maintain:

- Who has been invited?
- Who initiated the invitation?
- When this occurred?

In the email, rather than just inviting people in, we want to assign the user a key so that we can easily identify them on their way in. We will use a system-generated GUID to do this. In the case of inviting an existing user, we will allow him/her to log in to acknowledge the new friendship. In the case of a non-member user who was invited, we will allow him/her to create a new account. In both cases we will populate the invitation with the invited user's Account ID so that we have some history about the relationship.

## Adding friend alerts

Once we have the framework in place for inviting and accepting friendship requests, we need to extend our existing system with alerts. These alerts should show up on the existing user's Alerts page to show that they sent an invitation. For existing users, we should have alerts showing that they have been invited. Once a user has accepted a friendship we should also have a related alert.

## Interacting with your friends

Now let's discuss some of the features that we need to interact with our friends.

### Viewing your friends

Friends are only good if a user can interact with them. The first stop along this train of thought is to provide a page that allows a user to see all the friends he/she has. This is a jumping off point for a user to view the profile of friends. Also, as the concept of a user's profile grows, more data can be shown about each friend in an at-a-glance format.

In addition to an all **Friends** page, we can add friends' views to a user's public profile so that other users can see the relationships.

### Managing your friends

Now that we can see into all the relationships we can finally provide the users with the ability to remove a relationship, in our initial pass this will be a permanent deletion of the relationship.

### Following your friends

We can extend the alert system so that when alerts are generated for a user, such as updating their profile information, uploading a new photo, or any other user-specific task, all the user's friends are also notified via their Alerts page.

### Providing status updates to your friends

We have briefly touched on this already in earlier sections. Users can write a short message as their status update/micro blog and this will show up on the Alerts page of all the user's friends. This feature creates a lot of dynamic content on an end user's homepage, which keeps things interesting.

## Solution

Now let's look at our solution.

### Implementing the database

Let's look at the tables that are needed to support these new features.

### The friends table

As the concept of friends is our base discussion for this chapter, we will immediately dive in and start creating the tables around this subject. As you have read previously this is a very straightforward table structure that simply links one account to the other.

![](img/15a62e134135dde036b0ac3cbb01dab6_178_0.png)

### Friend invitations

This table is responsible for keeping track of who has been invited to the site, by whom, and when. It also holds the key (GUID) that is sent to the friends so that they can get back into the system under the appropriate invitation. Once a friend has accepted the relationship, their AccountID is stored here too, so that we can see how relationships were created in the past.

| Column Name | Data Type | Allow Nulls |
|---|---|---|
| InvitationID | int | ☐ |
| AccountID | int | ☐ |
| Email | varchar(200) | ☐ |
| GUID | uniqueidentifier | ☐ |
| CreateDate | smalldatetime | ☐ |
| BecameAccountID | int | ☐ |
| Timestamp | timestamp | ☐ |

### Status updates

Status Updates allow a user to tell their friends what they are doing at that time. This is a micro blog so to speak.

> A **micro blog** allows a user to write small blurbs about anything. Examples of this are Twitter and Yammer. For more information take a look here at: **http://en.wikipedia.org/wiki/Micro-blogging**.

### FriendRepository

This method queries all friends that are owned by this account. Note that the LINQ to Entity query returns IEnumerable<T>, where T is the class. We want to return List<Friend> which is achieved by calling friends.ToList().

It then queries for the reverse relationship where this account is owned by another account. Then it adds the second query to the first and returns that result.

Here is the method that gets the Accounts of our Friends.

```
//Fisharoo/DataAccess/Repositories/FriendRepository.cs
public List<Account> GetFriendsAccountsByAccountID(Int32 AccountID)
{
    List<Friend> friends = GetFriendsByAccountID(AccountID);
    List<int> accountIDs = new List<int>();
    foreach (Friend friend in friends)
    {
        accountIDs.Add(friend.MyFriendsAccountID);
    }

    List<Account> result = new List<Account>();
    using (FisharooDataContext dc = conn.GetContext())
    {
        IEnumerable<Account> accounts = from a in dc.Accounts
                                         where accountIDs.Contains(a.AccountID) select a;
        result = accounts.ToList();
    }
    return result;
}
```

This method first gathers all the friends (via the first method we discussed) and then queries all the related accounts and returns the result.

### FriendInvitationRepository

Like the other repositories this one has the standard methods and we don't list them here. Refer to the code associated with this chapter for details. In addition to those we also need to be able to retrieve an invitation by GUID or the invitation key that was sent to the friend.

```
//Fisharoo/DataAccess/Repositories/FriendInvitationRepository.cs
public FriendInvitation GetFriendInvitationByGUID(Guid guid)
{
    FriendInvitation friendInvitation;
    using (FisharooDataContext dc = conn.GetContext())
    {
        friendInvitation = dc.FriendInvitations.Where(fi => fi.GUID
                                              == guid).FirstOrDefault();
    }
    return friendInvitation;
}
```

This is a very straightforward query matching the **GUID** values. In addition to the above method we will also need a way for invitations to be cleaned up. For this reason we have the following method:

```
//Fisharoo/DataAccess/Repositories/FriendInvitationRepository.cs
public void CleanUpFriendInvitationsForThisEmail(FriendInvitation friendInvitation)
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        IEnumerable<FriendInvitation> friendInvitations
                              = from fi in dc.FriendInvitations
                                where fi.Email == friendInvitation.Email &&
                                fi.BecameAccountID == 0 &&
                                fi.AccountID == friendInvitation.AccountID
                                select fi;
        foreach (FriendInvitation invitation in friendInvitations)
        {
            dc.FriendInvitations.DeleteObject(invitation);
        }
        dc.SaveChanges();
    }
}
```

This method is responsible for clearing out any invitations in the system that are sent from account A to account B and have not been activated (account B never did anything with the invite). Rather than checking if the invitation already exists when it is created, we will allow them to be created time and again (checking each invite during the import process of 500 contacts could really slow things down!). When account B finally accepts one of the invitations all of the others will be cleared. Also, in case account B never does anything with the invites, we will need a database process that periodically cleans out old invitations.

### StatusUpdateRepository

The method of interest in this repository is the one that gets topN StatusUpdates for use on the profile page.

```
//Fisharoo/DataAccess/Repositories/StatusUpdateRepository.cs
public List<StatusUpdate> GetTopNStatusUpdatesByAccountID(Int32 AccountID, Int32 Number)
{
    List<StatusUpdate> result;
    using (FisharooDataContext dc = conn.GetContext())
    {
        IEnumerable<StatusUpdate> statusUpdates = (from su in dc.StatusUpdates
            where su.AccountID == AccountID
            orderby su.CreateDate descending
            select su).Take(Number);
        result = statusUpdates.ToList();
    }
    return result;
}
```

This is done with a standard LINQ to Entity query with the addition of the Take() method, that translates into a TOP statement in the resulting SQL.

### AccountRepository

With the addition of our search capabilities we will require a new method in our AccountRepository. This method will be the key for searching accounts.

```
public List<Account> SearchAccounts(string SearchText)
{
    List<Account> result = new List<Account>();
    using (FisharooDataContext dc = conn.GetContext())
    {
        IEnumerable<Account> accounts = from a in dc.Accounts
            where(a.FirstName + " " + a.LastName).Contains(SearchText) || a.Email.Contains(SearchText) || a.Username.Contains(SearchText)
            select a;
        result = accounts.ToList();
    }
    return result;
}
```
