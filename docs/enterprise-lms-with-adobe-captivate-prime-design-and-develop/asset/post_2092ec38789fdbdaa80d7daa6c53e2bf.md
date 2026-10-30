
In addition to these four roles, Captivate Prime also contains an *implicit* role: **Manager**. This role is considered implicit because it is automatically assigned to a user when his/her email address is supplied in the **Manager Email** field of another user. In Captivate Prime, a manager can manage the learning of his/her team members. For example, a manager can enroll team members in courses, approve or reject enrollment requests, generate reports, track performance, and more.

You will review the **Manager** role in more detail in *Chapter 6, Enrolling Learners to Courses*, and in *Chapter 9, Configuring and Using Feedback*.

In the next section, you will return to Captivate Prime and start assigning roles to the users you defined previously.

## Assigning roles to users

Now that you have a better understanding of the default roles available in Captivate Prime, let's return to the user list and assign roles to users:

-   1. Make sure you are logged in as an Administrator.
-   2. From the admin home page, click the **Users** link located in the **Manage** section of the left sidebar. This will take you to the **Internal Users** page.
-   3. Use the checkbox in the first column to select the **Instructor 01** user.
-   4. Click the **Actions** button located in the top-right corner of the screen and select the **Assign Role** action in the list that opens.
-   5. Click **Make Instructor** to assign the instructor role to the **Instructor 01** user.
-   6. Repeat these steps to also assign the **Author** role to the **Instructor 01** user.

As you do so, also notice the **Remove Role** option in the list of available actions. Taking a closer look at it, you will see that the learner role cannot be removed from a user. In other words, everyone in Captivate Prime is a learner!

Before moving on to the next section, let's quickly review the roles that can be assigned to external users.

-   7. From the **Internal Users** page, click the **External** link located in the **Users** section of the left sidebar.
-   8. Click the **Seats Used** link associated with the external registration profile you created in the *Creating external users* section, earlier in this chapter.
-   9. Use the checkbox to select the **Learner 03** user.
-   10. Click the **Actions** link located in the top-right corner of the screen.
-   11. Hover over the **Assign Role** action and notice that an external user can only be made an **Author** or a **Manager**.

With that, you've learned that an external user cannot become an **Administrator** or an **Integration Admin**. You also learned that, for external users, the **Manager** role is explicit, which is yet another difference between internal and external users. This makes a lot of sense since external users are outside your organization, so you don't have access to information about the role and hierarchy level of external users within their respective organizations.

Once designated as a **Manager**, the external user automatically becomes the manager of all the other *existing and future* external users using the same external registration profile. Also, note that you cannot remove the manager role from an external user.

The built-in roles of Adobe Captivate Prime should cover your basic needs, but there are many scenarios where they do not quite fit the bill. That's why Captivate Prime allows you to create custom roles, which is what will be covered in the next section.

## Creating custom roles

As an enterprise LMS, Adobe Captivate Prime should allow you to implement a large number of business requirements. For example, your organization might contract external eLearning agencies for creating eLearning content that must be uploaded to your Prime account. That's why these external contractors need Author access to your account, but you may want to prevent these external agencies from seeing each other's content. The problem is that, by default, every author has access to the entire Content Library. This is one of the situations where creating a custom role is essential.

To address this example, you need to create content folders using the steps outlined in the *Using content folders* section of *Chapter 3, Uploading Learning Content and Managing the Content Library*.

There are lots of other situations where creating custom roles is useful:

-   By default, a Prime Administrator is responsible for both the daily operation of your Prime account and implementing your organization's training strategy. Using custom roles, you can split the Administrator role into a *Platform Admin* role (the main configuration of your account, creating internal and external users, branding your account, and more) and a *Training Admin* role (enrolling users in courses, managing skills, badges and certifications, generating reports, and more).
-   You may want junior authors to only be able to upload new content to the Content Library so that assembling this content into courses remains a prerogative of senior authors. This can be achieved by creating a *Junior Author* custom role.
-   Etc.

Now that you have a better idea of the importance of custom roles, let's review the steps required to create one:

-   1. Make sure you are logged in as an Administrator.
-   2. From the Administrator home page, click the **Users** link located in the **Manage** section of the left sidebar to access the **Internal Users** page.
-   3. Then, click the **Custom Roles** link located at the end of the **Advanced** section in the left sidebar to access the **Custom Roles** page.
-   4. Click the **Create Role** link at the end of the **Roles** column.
-   5. Enter a name for the new role (*Junior author*, in this example) in the **Name of Role** field.
-   6. Next, you need to specify **Privileges** for the new role. For the *Junior Author* example, you would allow either **Full Control** or access to **Selected Folders** of the Content Library under the **Account Privileges** section.
-   7. Finally, use the **Search for users** field in the **Users** section at the very bottom of the form to select the users you wish to assign this custom role to.
-   8. When you're ready, click the **Save** button located in the top-right corner of the screen.

Take some time to browse all the privileges that can be included in a **Custom Role** and try to imagine how *your* organization could take advantage of all the possibilities offered by this feature.

Also, notice the **Scope for Feature Privileges** section, which allows you to restrict the privileges that are allocated to this role to specific catalogs and/or user groups (*catalogs will be discussed in Chapter 14, Working with Catalogs and Peer Accounts*).

Note that only internal users can be assigned such custom roles.

This wide range of possible combinations makes custom roles a very powerful feature of Captivate Prime. Of course, many complex scenarios are possible, which makes it impossible to cover them all in this book.

For more information on custom roles and privilege management, please refer to the following page, which is from the official Captivate Prime user guide: https://helpx.adobe.com/captivate-prime/user-guide.html/captivate-prime/administrators/feature-summary/custom-role.ug.html.

## Summary

At the end of this chapter, learning activities can finally start to occur on your Prime Account as all the needed ingredients are now properly in place:

-   In *Chapter 3, Uploading Learning Content and Managing the Content Library*, you created modules by uploading various types of learning activities to your Prime account.
-   In *Chapter 4, Creating Skills and Courses*, you assembled these learning modules into courses, and you assigned skills to courses.
-   Finally, in this chapter, you added the users that will bring your Prime account to life.

You have learned that Captivate Prime distinguishes between internal and external users. Typically, internal users originate from within the organization, while external users are individuals from third-party organizations who need access to your Prime account. Of course, feel free to tweak this typical distinction to meet your specific business requirements.

You have also reviewed several ways to create new internal and external users. Internal users can be created one at a time using the **Single User** option, in bulk using the **Upload a CSV** option, or through self-registration links that you can define as an administrator. For external users, it is necessary to create external registration profiles to allow these users to register themselves to the platform.

Maintaining an up-to-date user list is a real challenge, and the bigger the organization, the greater the challenge! Employees are constantly joining and leaving the organization, people change roles, teams are created and dissolved as the business evolves, acquisitions are made, branches are opened, merged, and closed, and more. This is the reason why most organizations maintain some kind of internal user directory using solutions such as LDAP (Microsoft Active Directory, OpenLDAP, and more), Salesforce, Workday, or any type of **Customer Relationship Manager (CRM)** or **Enterprise Resource Planner (ERP)** solution.
