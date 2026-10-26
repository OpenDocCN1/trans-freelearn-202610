# Deploying IIS 10.0 on Nano Server

+   7. Execute the command `Get-IISSite` in PowerShell, and you will get all the websites listed inside your IIS instance:

![](bbox=[0.105, 0.168, 0.895, 0.588])

+   8. You can see in the figure that we only have one default website, **Default Web Site** which has **State** as **Started** and a **Physical Path** of `%SystemDrive%=C:.` The system drive is the default operating system drive, `%SystemDrive%\inetpub\wwwroot`, and the binding is `http *:80:`, which means any IP available on Nano Server.

+   9. You can stop and start the website with the PowerShell commands `Stop-IISSite -Name "Default Web Site"` and `Start-IISSite -Name "Default Web Site"`, respectively.

+   10. You can do many more things in IIS through PowerShell; for more reference, you can use the URL `https://technet.microsoft.com/itpro/powershell/windows/iisadministration/iisadministration`.