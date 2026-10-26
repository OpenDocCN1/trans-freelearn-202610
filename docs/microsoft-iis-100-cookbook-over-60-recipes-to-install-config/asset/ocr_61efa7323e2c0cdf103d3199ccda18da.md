# How it works...

In this recipe, we remotely connected PowerShell with Nano Server to manage IIS over the IISAdministrator module. We used a Windows Server 2016 virtual/physical server instance for remote PowerShell management and creating a virtual directory on Nano Server 2016. We also created a website named `NanoAspnet` and created a virtual directory `MyApp` inside `NanoAspNet`.

# Configuring a virtual directory in IIS 10.0

In this recipe, we will create an application pool and assign it to our virtual directory, `MyApp`. We will create a firewall rule to allow inbound port `8000` for `NanoAspNet`.

# Getting ready

To step through this recipe, you will need a Windows Server 2016 virtual or physical server for remote PowerShell management and configuring a virtual directory on Nano Server 2016. For all this installation and configuration, you should have administrative privileges.

# How to do it...

+   1. Log in to Windows Server 2016. The IP is `172.16.15.60` and the name **WIN2016IIS**. Press *Windows + R* key. You will get the **Run** window. Open PowerShell ISE with administrative rights: type `RunAs /user:Administrator PowerShell_ISE.exe` and press **OK**.
+   2. Now, it will ask you the administrator password for your Windows Server 2016 (not Nano Server) machine. Type the password and press *Enter*. You will get the PowerShell window.
+   3. Now you need to remotely connect to Nano Server 2016 using the IP `172.16.15.63`. Write the command `Enter-PSSession -ComputerName "172.16.15.63" -Credential ~\Administrator.` You will get a popup window for the Nano Server administrator account password. Enter the password and press **OK**. You'll have connected successfully. Now you have to write `Import-module iisAdministration` and press *Enter*. This will import the PowerShell module.