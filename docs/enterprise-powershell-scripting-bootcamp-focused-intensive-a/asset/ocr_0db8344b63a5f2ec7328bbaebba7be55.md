Lastly, you will want to evaluate user profiles. In large enterprise environments, there can be few traces of what user or department owns a particular system. Since user profiles contain the usernames of individuals who logged into the system, scanning the profiles will provide guidance for system ownership.

In this chapter, you will:

+   - Understand how to get, stop, start, and set Windows services
+   - Leverage different cmdlets to interact with Windows processes
+   - Identify logged on users on a system
+   - Gather data about the user profiles on a system

> The examples in this chapter build upon each other. You will need to execute the scripts sequentially to have the final script in this chapter work properly.
> It is also recommended you execute these examples on Windows 2012 R2 server (or greater) with PowerShell installed. This server must be joined to a Windows domain.

# Windows services

When you are working with Microsoft-based systems, there may be times where you need to interact with Windows services. PowerShell offers a variety of cmdlets that enable you to work with these services. To start, you can review the services on a system by leveraging the Get-service cmdlet. By calling the Get-service cmdlet, you can retrieve the full list of Windows services on a system. If you want to obtain a filtered view into a specific service, you can leverage the -name parameter referencing a specific name of a service. After executing this command, you will see the Status, Name, and DisplayName of the service. You may also issue the -RequiredServices parameter to display the services that are required to be running for that particular service to be functional. You can query dependent Windows services by executing the -DependentServices parameter.

To use the Get-service cmdlet to query the Windows Audio Service, you could do the following:

```
```powershell
Get-service -DisplayName "Windows Audio"
Get-service -DisplayName "Windows Audio" -RequiredServices
(Get-service -DisplayName "Windows Audio").Status
```
```