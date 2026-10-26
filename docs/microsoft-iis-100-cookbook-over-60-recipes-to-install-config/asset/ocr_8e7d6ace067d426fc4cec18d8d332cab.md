# Uploading and testing web pages in a virtual directory

In this recipe, we will log in to Nano Server remotely. We will use PowerShell and check our created sites. We will access the Nano Server C$ share to upload our web pages. We will test the NanoAspNet website on port 8000 and also test the MyApp virtual directory.

# Getting ready

To step through this recipe, you will need a Windows Server 2016 virtual or physical server for remote PowerShell management and configuring virtual directory on Nano Server 2016. For all this installation and configuration work, you should have administrative privileges.

# How do to it...

+   1. Log in to Windows Server 2016. The IP is 172.16.15.60 and the name of the server is WIN2016IIS. Press Windows + R. You will get the Run window. We have to open PowerShell ISE with administrative rights. Type RunAs /user:Administrator PowerShell_ISE.exe in the Run window and press OK.
+   2. Now it will ask you the administrator password for your Windows Server 2016 (not Nano Server) machine. Type the password and press Enter. You will get the PowerShell window.
+   3. Now you have to remotely connect to Nano Server 2016 using the IP 172.16.15.63. Run the following command in PowerShell:
+   ```
+   "Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator"
+   ```
+   4. Now you will get a popup window for the Nano Server administrator account password. Enter the password and press OK to log in. You'll have connected successfully.