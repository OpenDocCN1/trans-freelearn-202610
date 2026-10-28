Upon successful login, on the admin dashboard, you can see that there is no link to Groups:

![](bbox=[0.115, 0.12, 0.85, 0.33])

Figure 4.23 – The admin dashboard

As we did not assign any group permissions, not even auth | group | Can view group, to the Help Desk User group, when Carol logs in, the Groups admin interface is not available to her. Similarly, when you navigate to Home | AUTHENTICATION AND AUTHORIZATION | Users and click a user link, you will see that there are no options to edit or delete the user. This is because of the permissions that were granted to the Help Desk User group, of which Carol is a member. The members of the group can view and edit users but cannot add or delete any user.

In this exercise, we learned how to grant a certain amount of administrative privileges to users of our Django app.

Now that we have an understanding of how the admin app works with the AUTHENTICATION AND AUTHORIZATION models for Users and Groups, we can turn our attention to registering the models that we have developed in the reviews app.

# Registering models with the admin app

The Django admin app produces workable CRUD interfaces to Django objects based on the characteristics of the models, with a minimal amount of coding. In this section, we will first look at how to register the models with the admin app and how elements of the interface, such as change lists and change pages, are derived from the model properties. We will conclude with an important exercise that demonstrates how foreign key settings in the model determine the behavior of the object deletion process.