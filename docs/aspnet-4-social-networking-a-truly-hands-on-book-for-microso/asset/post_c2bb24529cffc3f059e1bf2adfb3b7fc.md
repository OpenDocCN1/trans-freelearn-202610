
The values from the routed URL are added to the RouteData collection and are accessible from the Page class. We extract this value, cast it as a string, and then pass it to our presenter to take further action based on this value. The specific part of the presenter's Init() method, which is of interest here, is as follows:

```
if (string.IsNullOrEmpty(username))
    _accountBeingViewed = userSession.CurrentUser;
else
    _accountBeingViewed = accountService.
        GetAccountByUsername(username);
```

If the passed-in username is valid, then we query for that account and display it. That's all there is to URL routing and getting it to support our fancy URL requirement.

In case you are wondering, won't a direct call to the Profile.aspx page (http://www.fisharoo.com/profiles/profile.aspx) match the URL route we have defined, with the username parameter being set to profile.aspx? The answer is no. The routing engine by default excludes all calls that match to physical files.

## News feed/Alerts

The last concept that we will cover in our discussion about building profiles is news feed (or alerts). A news feed allows us to see what we have been doing with our account, friends, our photos, and any other changes in the system. More importantly, it will allow us to see what our friends have been doing with all their data, such as when they add a new photo.

This system will be built on the alerts system that we have already discussed. Any time we want to add a new alert to the system based on an action performed by our user or the system, we simply make a call into the AlertService and select the appropriate method such as AddNewAvatarAlert(). Once the site is peppered with the addition of alerts to the system, we will have a fairly active news feed to follow. Now, we just need a place to read this feed!

Knowing that this news feed will easily be one of the most active pages on our site, we should put it on the users' homepage so that they can see it as soon as they log in. To implement this, all we need is a repeater with a label in it, on the homepage, which we can hook up to a list of alerts for the current user. See the code for //Fisharoo/Web/Profiles/Default.aspx page.

Then from the presenter, we need to pass the view the appropriate data to bind to. We get the list of alerts, convert them into a list of strings, and pass them to the view to display.

```
//Fisharoo/Web/Profiles/Presenters/DefaultPresenter.cs
private void ShowDisplay()
{
    List<string> msgs = new List<string>();
    List<Alert> alerts = _alertService.GetAlertsByAccountID(
        _userSession.CurrentUser.AccountID);
    foreach (Alert item in alerts)
    {
        msgs.Add(item.Message);
    }
    _view.ShowAlerts(msgs);
}
```

Once we get into the concept of *Friends*, this page will be much more active, as users can see their alerts as well as their friends'!

## Summary

We have covered a lot of ground in this chapter. We have discussed the concept of a profile and a way for our users to manage their data. We have also built a way for our users to manage their privacy regarding their profile data. We have also created tools for our users to manage a custom avatar as well as a way to hook up to their centrally stored Gravatar. With all of these features out of the way, we have created a public profile for our users. And finally, we also created a default landing page for users where they can see their alerts.

## 5

### Friends

The subject of this chapter is the key to the success of any community. Your friend features will be the main reason for people to interact with your community. It is also the biggest drive for your users to advertise for you. "Hey Peter, come and check this out. I love it, so will you!" — a friend in a community site, like in life, is someone who you have something in common with, enjoy the company of, or turn to when you have something to discuss. A circle of friends can be thought of as a sub-community within a community.

This chapter will show you how to take advantage of people's nature to congregate around things that they enjoy, find useful, or that intrigue them. We will start this chapter by extending the framework to allow for relationships to be built between our users. We will then add some features that allow our users to locate all of their existing friends, as well as make new friends. Then we will have a discussion about providing real-time status updates in the form of a micro blog so that all of our friends can stay in touch with everything we are doing.

### Problem

There are many aspects to building relationships in any community — real or virtual. First and foremost is initiating contact with the people whom you will eventually call your friends. The easiest way to locate someone who you might want to call a friend is to directly invite the people who you already know as a friend. We will do this in a few ways.

- 3. Third, we will create a form that allows you to import all of your contacts from Outlook.

![](img/15a62e134135dde036b0ac3cbb01dab6_173_0.png)

All of these methods of inviting a friend into the system would of course generate an email invite (and eventually a system-based message—see next chapter). The user will have the ability to then follow the link into the system and either sign up or log in to accept the request. Once the user has clicked on the link in their email, he/she will be taken to a page displaying the request.

![](img/15a62e134135dde036b0ac3cbb01dab6_173_1.png)

![](img/15a62e134135dde036b0ac3cbb01dab6_175_0.png)

The box in the preceding screenshot is where the user can enter his/her **Status Updates**. Each of these updates will also be shown on the updates view and in their alerts view.

![](img/15a62e134135dde036b0ac3cbb01dab6_175_1.png)

Such alerts/notifications keep the people aware of what their friends are up to and keep them involved with their friends.

### Design

Now let's talk about the design of these features.

### Friends

This chapter is an attempt to throw light on the infrastructure needs and more heavily focus on the UI side for creating and managing relationships. That being said, there is always some form of groundwork that has to be in place prior to adding new features.

In this case we need to add the concept of a friend prior to having the ability to create friendships. This concept is a relatively simple one, as it is really only defining a relationship between two accounts. We have the account that requested the relationship and the account that accepted the relationship. This allows an account to be linked to as many other accounts as they wish.

### Finding friends

The Friends feature will not go anywhere if there is no way for people to find friends. For that reason, the various ways to locate and invite someone to be your friend is our first topic.

### Searching for a friend

The easiest way to locate friends who might be interested in the same site as you are is to search through the existing user base. For that reason, we will need to create a simple keyword search box that is accessible from any page on the site. This search feature should take a look at several fields of data pertaining to an account and return all possible users. From the search results page, we should be able to initiate a friend request.

### Inviting a friend

The next best thing to locating friends who are already members of the site is to invite people who you know outside the site. The quickest way to implement this is to allow a user to manually enter an email address or many email addresses, type a message, and then submit. This would be implemented with a simple form that generates a quick email to the recipient list. In the body of the email will be a link that allows the recipients to come into our site.

### Importing friends from external sources

An obvious extension of the last topic is to somehow automate the importing process of contacts from an email management tool. We will show how the user can export their contacts from Outlook and import them via a web form. The user should then be able to select the contacts that they want to invite.
