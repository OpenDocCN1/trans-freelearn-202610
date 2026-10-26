7. Once we run the command, we will get a list of application pools available in IIS on Nano Server, the PowerShell command will only recycle the application pool **NanoAPP**, and we'll get its status as Started. We can use the start and stop commands `$sm = Get-IISServerManager` `$sm.ApplicationPools["NanoAPP"].Stop()` and `$sm.ApplicationPools["NanoAPP"].Start()` for the application pool, respectively. We can use these commands as and when we require.

8. Now we are going to check the default document files list available in the **Default Document** settings. Write the command `Get-IISConfigSection -SectionPath "system.webServer/defaultDocument" | Get-IISConfigCollection -CollectionName "files"` in PowerShell and press Enter, as shown here:

![](bbox=[0.1, 0.36, 0.89, 0.83])