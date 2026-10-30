
Notice that, by default, external users do not have to create an Adobe ID to access your Prime account. They must also log in to Captivate Prime using a special login page that is emailed to them when they register. These are some other important differences between internal and external users. In the Configuring login methods section, later in this chapter, you will review the options available to decide on the login methods for both internal and external users.

To conclude this section, let's return to the administration area of your Prime account to review the options that allow you to administer external users.

## Administering external users

Now that you have enabled an external user profile and a user has used that profile to register to your Prime account, you will go back to the administration area and review the options that allow you to administer external users:

- 1. Return to the Administrator application of your Prime account.
- 2. From the Administrator home page, click the Users link located in the Manage section of the left sidebar.
- 3. Next, click the External link in the Users section of the left sidebar. This will take you to the External Users page, where the external registration profile you created earlier is visible in the list.

From here, a few key pieces of information about the external user profile are available at a glance. As shown in the following screenshot, you can see the number of Seats Used, the Expiry date, Login Requirement, as well as Status:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_194_0.png)

Figure 5.15 – Key information about external user profiles is available at a glance

- 4. Click the external user profile name visible in the first column of the list. This opens the Edit External Registration Profile form, which you can use to modify the information about the external registration profile.
- 5. Click the Cancel button to close the Edit External Registration Profile form and return to the External Users page of the administrator application.
- 6. Click Seats Used to access a list of users that have registered themselves using this external registration link.

- 7. Click the **External** link located in the **Users** section of the left sidebar to return to the **External Users** page.
- 8. Use the checkbox in the first column of the **External Users** page to select the registration profile you created previously.
- 9. Click the **Actions** button in the top-right corner of the screen to review the available actions.

There should be two actions available in the list that opens: the **Pause** action and the **Resume** action. Use the **Pause** action to temporarily disable the ability to create new users via this external registration profile. Note that existing users retain their ability to log in to your Prime account, even though the external registration profile has been paused. The **Resume** action is used to restore the ability for external users to register themselves using this external registration profile. This feature is useful for seasonal staff who need to register during a given period.

- 10. Click the **Status** toggle button to toggle the external registration profile off.

When the external registration profile is turned off, two things happen. First, it is no longer possible to create new users via that external registration profile, and second, existing users that were created via that external registration profile lose their ability to log in to your Prime account. This is the nuance that makes deactivating an external registration profile different from pausing it. Toggling an external registration profile off is useful in situations such as the end of a partnership, the end of a training agreement, the acquisition or closure of an external partner, and so on.

- 11. Before moving on to the next section, make sure you reactivate the external registration profile by turning its **Status** back on.

With that, you have created all the users you need to continue with the exercises in this book. Before moving on, we'll provide a quick summary of the users you created in the last few sections:

- You should have five users, in addition to your original Administrator profile.
- Four of those users are internal users, the fifth one is an external user.
- The internal users are named Instructor 01, Learner 01, Learner 02, and Manager 01. The external user is named Learner 03.
- At the moment, all these additional users are learners. You have not assigned special roles to any of them yet.
- You have also successfully tested the ability to log in as these users.

Creating these users has given you a comprehensive overview of the various ways to create users in Captivate Prime. You have manually created a user using the **Single User** option, you have imported users using the **CSV Import** option, and you have created **Self-Registration** links for internal users, as well as an **External Registration Profile** for external users.

If you have met these requirements, then you are now ready to move on to the next section, where you will learn how to customize the user form to grab custom pieces of data about the users of your Prime account.

## Customizing the user form

When creating users in the previous sections, you only had to provide very basic user information. A username, email address, and password are all that it takes for Captivate Prime to grant users access to the platform.

This is in contrast to most corporate directories, which contain much more information about employees. Data such as department, employee ID, work location, and so on are very common. This additional data often plays an essential role in the daily activities of an organization. Depending on various factors, such as the size of the organization, its business model, its complexity, and so on, essential elements such as business reporting, human resource management, asset management, localization of services, and more are largely based on this additional information. Moreover, the way this information is organized, as well as its content, is specific to each organization.

As an enterprise LMS, Captivate Prime understands the importance this additional metadata has to your organization. So, it gives you the ability to capture this data and use it in different areas of the system, such as user management, skills management, learner enrollment, reporting, and so on.

In this section, you will take a look at **Active Fields**, which allows you to define additional custom metadata for each user. You will also discover how this affects the user creation process and the learner experience:

- 1. Make sure you are logged in as an Administrator.
- 2. From the Administrator homepage, click the **Users** link located in the **Manage** section of the left sidebar. This will take you to the **Internal Users** page.
- 3. From there, click the **Active Fields** link located in the **Advanced** section of the left sidebar.

This last action takes you to the **Active Fields** page, where you can define additional fields for both internal and external users, as shown in the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_197_0.png)

Figure 5.16 – The Active Fields page is where you can define additional fields for both internal and external users

Notice that, for internal users, three additional fields (the **Employee ID**, **Department**, and **Location** fields) have already been defined (see the arrow in the preceding screenshot).

## Activating and customizing CSV fields

These three extra fields correspond to the three unmapped columns of the CSV file that you imported in the *Importing users from a CSV file* section, earlier in this chapter. By default, none of these three extra fields are active. Activate and customize two of these fields by performing the following steps:

- 1. Select the checkboxes associated with the **Department** and **Location** fields to activate them.
- 2. Click the **Modify Values** link to open the **Values in Custom Fields** form.
- 3. Open the **Select Field** drop-down list and choose the **Department** field.
- 4. Select the **Restrict Selection** checkbox.
- 5. Open the **Select Field** drop-down list one more time and choose the **Location** field.
- 6. Also, select the **Restrict Selection** checkbox for the **Location** field.
- 7. Click the **Done** button located in the lower-right corner of the form to return to the **Active Fields** page.
- 8. Also, click the **Save** button located in the top-right corner of the **Active Fields** page.

Thanks to these steps, you have activated the **Department** and **Location** fields. This means that these two pieces of data can now be collected by Captivate Prime and associated with each user. To check this out, let's return to the **Internal Users** page to modify an existing user.

- 9. Click the **Internal** link located in the **Users** section of the left sidebar.
- 10. Click your name in the list of internal users to open the details about your user account.
- 11. Click the pencil icon located in the top-right corner of your user's details to open the **Edit User** form.

As shown in the following screenshot, the **Department** and **Location** fields have been added at the bottom of the standard Captivate Prime user form:
