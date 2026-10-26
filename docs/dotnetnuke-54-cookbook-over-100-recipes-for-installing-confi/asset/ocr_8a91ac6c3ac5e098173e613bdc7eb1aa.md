24. For **User Name**, type `sysadmin` (don't use the standard `admin` for the username. It is too well known).

25. Choose a good password and confirm it. Then write it down as well.

26. Provide your e-mail address to get system messages.

27. Lastly, the wizard will prompt for the basic information for your site. Choose a title for your site and type it into **Portal Title**.

28. Select **Default Website** for the **Template**.

29. Click on Next.

30. When you see the Congratulations screen, click on **Finished** (Goto Site) to complete the installation.

31. After a pause you should see the Welcome screen to your new website. At this point you have successfully created a debugging DNN website.

# Preparing a module for debugging

Once we have created a debugging environment, we can add our modules to the DNN website project and debug our module in the DNN environment.

# Getting ready

To follow along with this recipe you must have completed these recipes

+   - *Setting up a debugging environment*
+   - *Deploying a module as a standalone package*

# How to do it...

+   1. Start by launching your preferred Development Tool (Visual Studio or Visual Web Developer Express).
+   2. From the **File** menu, select **Open Web Site**.....
+   3. At the **Open Web Site** dialog, select the DNN source folder (`My Documents\DNNSource\website`).
+   4. Now we will add our existing Employee project. Look under the **File** menu and select **Add/Existing Project**.....
+   5. Browse to the Employee project folder (probably `My Documents\Visual Studio 2010\Projects\Employee\Employee`) and select the `Employee.vbproj` file.