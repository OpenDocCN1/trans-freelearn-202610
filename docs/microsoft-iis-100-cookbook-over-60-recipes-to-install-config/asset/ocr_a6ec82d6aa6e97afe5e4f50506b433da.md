+   13. Select **All Users** and provide **Read** and **Write** permissions. Click on **OK**. Check the **FTP Authorization Rule**. If there are any other rules available, you can delete them. We will use the rule we created.

+   14. Next, go to the ftpsite **Features View** and open IIS Manager Permission. Here, you can allow the user to access the FTP site. Click on **Allow User...** from the **Actions** pane. You will get the **Allow User...** window, as shown here:

![](bbox=[0.285, 0.246, 0.715, 0.454])

+   15. We selected the `ftpadmin` user from the **WIN2016IIS** server. Click on the **OK** button.

# How it works...

In this recipe, we created a user on WIN2016IIS for FTP access and granted the FTP user full permissions to the `ftproot` folder. We configured the FTP authentication and FTP authorization rules for all users. We allowed the Windows user to access the FTP site from IIS Manager Permission.

# Testing our FTP server

In this recipe, we are going to test the FTP site called `ftpsite` using FileZilla. We will create the FTP site in FileZilla and connect to `ftpsite` on WIN2016IIS. We will upload a sample file and cross-check it in the `ftproot` folder.