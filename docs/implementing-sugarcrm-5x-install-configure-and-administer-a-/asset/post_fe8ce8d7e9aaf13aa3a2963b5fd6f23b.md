

## Password Management

Save Cancel

### System-Generated Passwords

Enable System-Generated Passwords Feature (1)

System-Generated Password Expiration

- None

Password Expires in Days Password Expires upon Logins

### User Reset Password

Enable Forgot Password feature (1)

Generated Link Expiration: (2)

- None

Link Expires in 30

Enable reCAPTCHA Validations (1)

### Email Templates

Email template containing system-generated password

System-generated password email Create Edit

Email template containing system-generated link to reset password

Forgot Password email Create Edit

### LDAP Support

Enable LDAP Authentication (1)

## Using Studio

As an administrator, you will undoubtedly find yourself in a situation where you need to customize your SugarCRM installation. Fortunately, SugarCRM provides a number of tools that serve this purpose.

The Administration home page shows a number of tools grouped together as **Developer Tools**. They include the following:

- Studio
- Dropdown editor
- Module builder
- Module loader
- Rename tabs

These tools collectively enable administrative users to perform a broad range of customization tasks, reducing the need for custom software development. For more information on these tools, refer to _Appendix E_, which discusses these SugarCRM customization tools in detail.

## Defining system e-mail

System e-mail settings serve a variety of purposes. The most basic use is to send e-mail notifications to users upon the assignment of records. It can also be used to monitor group e-mail inboxes, such as support@example.com or sales@example.com. Lastly, e-mail settings must be defined if you intend to send out mass e-mail through the built-in marketing campaign tool.

To configure e-mail settings, click on **Email Settings** on the administration screen. On this page, you will be able to define the mail transfer agent (sendmail or SMTP) for your installation, as well as enable the sending of notification e-mails to users by enabling the **Assignment Notifications** option. Note that your options will vary depending on the version of SugarCRM that you are using, as Version 5.5 and higher do not allow the use of **sendmail** by default.

Most Linux environments include **sendmail**, but SMTP is quite often an option as well. If you are unsure about which choice to make, consult your hosting provider (for hosted deployments) or review your server's e-mail services configuration.

If you would like to configure SugarCRM to monitor a group mailbox, click the **Inbound Email** option on the administration screen. Next, click on **Monitor New Mail Account** to enter the e-mail inbox settings, such as the server address, username, and password. Make sure to set a value for the **Assign To Group Folder** field. Once operational, SugarCRM will automatically retrieve e-mails for that account, and add them to the assigned folder. To access the e-mails, users can add the folder to their e-mail configuration and they will be automatically displayed.

Notice that the configuration screen also allows you to automatically create a support case every time an e-mail is retrieved from the specified e-mail inbox.

Perhaps one of the most common uses for the e-mail configuration is for the purposes of sending mass e-mails through the marketing campaign tool built into SugarCRM. If you are using SugarCRM within a hosted environment, you should check with your provider about any limitations or considerations that need to be observed with regard to the amount of e-mail you are allowed to send. In addition, you should investigate if any legislation in your area aims at controlling who you can send e-mail to or under which conditions.

To use e-mail campaigns within SugarCRM, the administrator must first enable the **System Scheduler** and configure the **Schedule list** within the **Scheduler** option within the System section of the administration home page.

The system scheduler is enabled in two different ways, one for a Linux server and another for a Windows server. For Microsoft Windows, you can use the task scheduler. For Linux, you can use cron.

For Linux, as a root user, type the following command at the shell prompt, replacing the _path-to-sugar_ with your own path to the Sugar installation directory and replacing the _apache_ with the proper username that the web server runs as (usually defaults to **apache** or **wwwrun**):

```
/echo "0,10,20,30,40,50 * * * * cd </path-to-sugar>;<path-to-php> ../scheduler.php" | crontab -u apache/
```

This will setup a cron job to check every 10 minutes whether any e-mail needs to be sent out. If there are any, the mass e-mail queue manager will process the template and send an e-mail out immediately to the recipient. Further information on the **cron** utility can be found at http://en.wikipedia.org/wiki/Cron.

For Windows, you will first need to create a batch file that causes Windows to execute PHP and the script relevant to the SugarCRM scheduler functionality. Usually, it is a batch file with something similar to the following:

```
cd C:\xampple\htdocs\sugarc55\php.exe -f cron.php
```

Change `C:\xampple\htdocs\sugarc55\` to reflect the appropriate directory that represents the location of your SugarCRM installation and paste into a text file named `SugarScheduler.bat`. Take note of where on your computer it is that you saved `SugarScheduler.bat` as we will need that information in the following step.

Next, select **Start | Settings | Control Panel | Scheduled Tasks**. Double-click on **Add Scheduled Task**. When the **Scheduled Task Wizard** asks you for the program you want Windows to run, browse for the `SugarScheduler.bat` file. Continue with the rest of the Wizard, making sure you click on the **Daily** option when asked when to perform this task. Note that you may need to login to Windows as admin level user in order to be able to complete this process.

Before you click on the **Finish** button for the Scheduled Task Wizard, check the box that says, **Open advanced properties for this task when I click Finish**. A new dialog box displays after you click on **Finish**. Click on the **Schedule** tab, and then on the **Advanced** button. Check the box for **Repeat task** and specify every 10 minutes within a duration of 24 hours.

## Recurring administration duties

This section describes those administrative duties that need to be performed on a regular or ad hoc basis throughout the life of your SugarCRM Community Edition installation.

## User management

New users will need to be created and existing users may need to be marked temporarily as **Inactive** if they take prolonged breaks, such as a maternity leave. You can see how to add a new user earlier in this section.

To mark a user as **Inactive**, go to the **Admin** main screen, select **User Management**, choose the user you wish to mark as **Inactive**, and click on the **Edit** button. Set the **Status** field to **Inactive** and then click on **Save**.

As mentioned previously, users may not be removed. Typically, the best solution is to mark a user as "Inactive" for a period of time (say six months) and then rename the user.

## Resetting passwords

If you need to reset a user's password, go to the **Admin** main screen, select **User Management**, and then select the corresponding user. On their detail view screen, click on **Edit**, and then define the new password. Proceed to click on **Save** when finished.

You will then need to e-mail or telephone the user to inform them of their new temporary password. It is also helpful to provide them with instructions on changing their password once logged into SugarCRM.

Alternatively, you may choose to turn on the **Enable Forgot Password feature** in the **Password Management** panel. Doing so would allow users to reset their password without needing to bother the administrator with the task.

## General maintenance

There are six topics that fall under general maintenance of the SugarCRM system. They are as follows:

- Updating currency rates
- Maintaining security roles
- Checking for and installing software updates
- System backups
- Data backups
- Monitoring

Some of these topics, such as the maintenance of currency rates and roles are explained sufficiently in the preceding section of this chapter, _Administration duties at system installation time_. However, let us explore the others.

