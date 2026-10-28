You will be presented with a confirmation screen and taken back to the Select user to change list once you have deleted the object:

![](bbox=[0.12, 0.15, 0.82, 0.35])

Figure 4.12 – User deletion confirmation

You will see the following message once the user is deleted:

![](bbox=[0.14, 0.43, 0.82, 0.48])

Figure 4.13 – User deletion notification

After that confirmation, you will find that David's account no longer exists.

So far, we have learned how we can add a new user, get the details of another user, make changes to the data for a user, and delete a user. These skills helped us cater to Alice, Bob, and David's requests. As the number of users of our app grows, managing requests from hundreds of users will eventually become quite difficult. One way around this problem would be to delegate some of the administrative responsibilities to a selected set of users. We'll learn how to do that in the section that follows.

# Managing Django users and groups

Django's authentication model consists of users, groups, and permissions. Users can belong to many groups, which is a way of categorizing users. It also streamlines the implementation of permissions by allowing permissions to be assigned to collections of users as well as individuals.

In Exercise 4.01, Creating a superuser account, we saw how we could cater to Alice, David, and Bob's requests to make modifications to their profiles. It was quite easy to do, and our application seems well equipped to handle their requests.