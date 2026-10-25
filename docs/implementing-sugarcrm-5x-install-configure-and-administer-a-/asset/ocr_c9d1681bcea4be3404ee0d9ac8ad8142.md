# # The System Administrator Role

You can define roles for groups of users and specify which modules those users should
have access to. For example, sales staff will want access to the opportunities and
dashboard modules, marketing staff will want access to the campaigns module, but
not all staff will need access to these modules. As you can see in the following figure,
each role defines the modules which will be visible to the users who are assigned the
specific role, as well as the capabilities the user will have within each module.

![](bbox=[0.104, 0.192, 0.927, 0.474])

To create a role, click on the **Create Role** shortcut, type a name and a description for
the role in the **Name** and **Description** fields respectively, and click on **Save**. Adjust
the settings to control the modules that the role can access and the functions that may
be performed within each module. Note that when a module is excluded from a role,
access is also removed to the subpanels in other modules that relate to the excluded
module. Click on **Save** to preserve those settings for your role.

To assign users to a role, use the detail view for that role. In the **Users** subpanel at
the bottom of the screen, click on the **Select** button to display a list of users. You can
check the usernames that you want to assign to this role. If a user belongs to multiple
roles, their access in each module is defined by the most restrictive access any of their
roles are assigned.

It is very important that you understand that by default, all users will have access
to all the data and all the modules. The use of roles in conjunction with the use of the
**Assigned To** field on individual records is the primary mechanism for controlling
access to modules and their data. If you need to control the access of specific users to
certain records, you should define your roles and assign them to the corresponding
users before you allow users access to the system.

[ 290 ]