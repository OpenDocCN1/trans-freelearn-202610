# # Sending an invitation

With all three of the above methods we will end up sending out an invitation email. We could simply send out an email with a link to the site. However, we need to maintain:

+   - Who has been invited?
+   - Who initiated the invitation? and
+   - When this occurred?

In the email, rather than just inviting people in, we want to assign the user a key so that we can easily identify them on their way in. We will use a system-generated GUID to do this. In the case of inviting an existing user, we will allow him/her to log in to acknowledge the new friendship. In the case of a non-member user who was invited, we will allow him/her to create a new account. In both cases we will populate the invitation with the invited user's Account ID so that we have some history about the relationship.

# ## Adding friend alerts

Once we have the framework in place for inviting and accepting friendship requests, we need to extend our existing system with alerts. These alerts should show up on the existing user's Alerts page to show that they sent an invitation. For existing users, we should have alerts showing that they have been invited. Once a user has accepted a friendship we should also have a related alert.

# ## Interacting with your friends

Now let's discuss some of the features that we need to interact with our friends.

# ## Viewing your friends

Friends are only good if a user can interact with them. The first stop along this train of thought is to provide a page that allows a user to see all the friends he/she has. This is a jumping off point for a user to view the profile of friends. Also, as the concept of a user's profile grows, more data can be shown about each friend in an at-a-glance format.

In addition to an all **Friends** page, we can add friends' views to a user's public profile so that other users can see the relationships.