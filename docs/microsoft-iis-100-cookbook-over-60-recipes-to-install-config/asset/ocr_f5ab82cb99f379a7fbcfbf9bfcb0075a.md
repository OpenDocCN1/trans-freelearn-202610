+   8. Log in to Windows Server 2016. The IP is `172.16.15.60` and the name of the server is **WIN2016IIS**. Press *Windows + R* on your keyboard, and you will get the *Run* window. Open PowerShell ISE with administrative rights, type in the **Run** command window `RunAs /user:Administrator PowerShell_ISE.exe`, and press **OK**.

> In this command, `Administrator` is the username for that machine; you can use any username that has administrator rights to perform the given task.

+   9. Now it will ask you the administrator password for your Windows Server 2016 (not Nano Server) instance. Type the administrator password and press *Enter*. You will get the PowerShell window.

+   10. Now you have to remotely connect to Nano Server 2016 at `172.16.15.63`. Run the command `Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator`. Now you will get a popup window for the Nano Server administrator account password. Enter the password and press **OK** to log in. You have now connected successfully. Write `Import-module isAdministrator` and press *Enter*. This will import the PowerShell module.

+   11. Now you need to install the Reverse Forwarders packages, which you created in the `Packages` folder in the Nano Server `C:\` drive, and copy the Nano Server Packages, as shown here:

![](bbox=[0.129, 0.589, 0.871, 0.867])