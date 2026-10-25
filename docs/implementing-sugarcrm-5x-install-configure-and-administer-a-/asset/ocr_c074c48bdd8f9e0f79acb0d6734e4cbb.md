For Linux, as a root user, type the following command at the shell prompt, replacing the _path-to-sugar_ with your own path to the Sugar installation directory and replacing the _apache_ with the proper username that the web server runs as (usually defaults to **apache** or **wwwrun**):

```
`/echo "0,10,20,30,40,50 * * * * cd </path-to-sugar>;<path-to-php> ../scheduler.php" | crontab -u apache/`
```

This will setup a cron job to check every 10 minutes whether any e-mail needs to be sent out. If there are any, the mass e-mail queue manager will process the template and send an e-mail out immediately to the recipient. Further information on the **cron** utility can be found at http://en.wikipedia.org/wiki/Cron.

For Windows, you will first need to create a batch file that causes Windows to execute PHP and the script relevant to the SugarCRM scheduler functionality. Usually, it is a batch file with something similar to the following:

```
`cd C:\xampple\htdocs\sugarc55\php.exe -f cron.php`
```

Change `C:\xampple\htdocs\sugarc55\` to reflect the appropriate directory that represents the location of your SugarCRM installation and paste into a text file named `SugarScheduler.bat`. Take note of where on your computer it is that you saved `SugarScheduler.bat` as we will need that information in the following step.

Next, select **Start | Settings | Control Panel | Scheduled Tasks**. Double-click on **Add Scheduled Task**. When the **Scheduled Task Wizard** asks you for the program you want Windows to run, browse for the `SugarScheduler.bat` file. Continue with the rest of the Wizard, making sure you click on the **Daily** option when asked when to perform this task. Note that you may need to login to Windows as admin level user in order to be able to complete this process.

Before you click on the **Finish** button for the Scheduled Task Wizard, check the box that says, **Open advanced properties for this task when I click Finish**. A new dialog box displays after you click on **Finish**. Click on the **Schedule** tab, and then on the **Advanced** button. Check the box for **Repeat task** and specify every 10 minutes within a duration of 24 hours.

# # Recurring administration duties

This section describes those administrative duties that need to be performed on a regular or ad hoc basis throughout the life of your SugarCRM Community Edition installation.