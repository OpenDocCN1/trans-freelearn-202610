The preceding screenshot displays one of the multiple messages generated when the win32_product class is invoked. You will see that the Windows installer forces a reconfiguration of the installed software product. You will also see a reconfiguration status and error status message. If you accidentally use the win32_product class, you can scan the event log for status messages and properly reinstall the application upon error.

![](bbox=[0.13, 0.21, 0.22, 0.28])

> Avoid using the win32_product class in your scripts. Since the MSI performs a repair/reconfiguration of the installed software, this will create instabilities in your environment. Use the other methods in this chapter to identify software installed on the system.

Due to the limitations with the win32_product class, you will need to access multiple areas of the operating system to identify installed applications. The first place to start is the registry. Since MSIs, and other software installers, place the logic for uninstalling the application in the registry, you can identify installed software in the HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\ and HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\ registry key locations. To identify those software titles, you have to loop through each sub-key and search for the property: DisplayName. If the key does not have a DisplayName property, it is highly likely that you need to discover the installed software by leveraging alternative methods.

To identify installed software by using the registry, you can perform the following:

```
$RegLocations = "HKLM:\Software\Microsoft\Windows\CurrentVersion\
Uninstall\*","HKLM:\Software\Wow6432Node\Microsoft\Windows\
CurrentVersion\Uninstall\*"

foreach ($reg in $RegLocations) {
    $softwareKeys = get-ItemProperty $Reg | Select DisplayName | Sort DisplayName
    foreach ($software in $softwareKeys) {
        if ($software.DisplayName -ne $null) { Write-host "Software Found: " $software.DisplayName }
    }
}
```