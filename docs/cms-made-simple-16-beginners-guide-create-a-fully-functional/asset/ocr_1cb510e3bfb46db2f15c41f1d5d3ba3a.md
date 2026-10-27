# Creating Your Own Functionality

These two lines would show the comment form and the summary view of all comments written by one user. When the user is not logged in, he cannot see the comments, as the variable $userid does not exist for users not logged in. Another user would not see these comments as well, as he has another user ID. Test it by placing these two Smarty tags into the page Service Desk. As there is a login form on the page, the complete content should look similar to the following screenshot:

![](bbox=[0.219, 0.273, 0.785, 0.488])

A logged in user can now create comments or, let's say, support tickets, which can only be viewed by himself on the page or by the support staff in the admin console of CMS Made Simple. You can fully customize the template with the comment form, as shown in the following screenshot:

![](bbox=[0.285, 0.59, 0.718, 0.862])