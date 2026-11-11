> To query Windows services requires a PowerShell window running with elevated privileges. You will need to open PowerShell with the Run as administrator parameter.

To query the Windows Audio Service for the user delegated to start the service, you can perform the following:

```
$service = get-wmiobject win32_service | where {$_.DisplayName -like "Windows Audio"}
$servicedisplay = $service.DisplayName
$serviceAuthUser = $service.StartName
write-host "Service with $servicedisplay name is running with $serviceAuthUser account."
```

The output of this is shown in the following screenshot:

![](bbox=[0.07, 0.38, 0.9, 0.42])

This example displays how to determine the owner of services on a system. You first start by calling the get-wmiobject cmdlet with the -class parameter set to win32_service. This command is piped to where {$_.DisplayName -like "Windows Audio"}. The Windows Audio object is then placed into the $service variable. You gather the Windows service display name by calling the $service.DisplayName property and place the property into the $servicedisplay variable. You retrieve the Windows service start-up user by calling the $service.StartName property and place the property into the $serviceAuthUser variable. To display the collected data, you use the write-host cmdlet with Service with $servicedisplay name is running with $serviceAuthUser account.. On this particular system, Service with Windows Audio name is running NT Authority\LocalService account. is returned.