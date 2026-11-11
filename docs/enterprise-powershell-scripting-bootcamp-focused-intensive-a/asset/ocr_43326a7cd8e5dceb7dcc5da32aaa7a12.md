To query the win32_logicaldisk class for disk information, you can perform the following:

```
Get-WmiObject -class win32_logicaldisk
```

The output of this is shown in the following screenshot:

![](bbox=[0.25, 0.17, 0.71, 0.74])

The following example displays how to leverage get-wmiobject to query WMI to provide disk information:

+   1. You start by leveraging the get-wmiobject cmdlet with the -class parameter set to the win32_logicaldisk argument.
+   2. After execution, the cmdlet returns the DeviceID, DriveType, ProviderName, FreeSpace, Size, and VolumeName properties.