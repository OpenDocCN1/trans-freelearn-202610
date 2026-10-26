12. Type the dism/online/addpackage/packagepath:c:\packages\Microsoft-OneCore-ReverseForwarders-Package.cab and dism /online /add-package /packagepath:c:\packages\en-us\Microsoft-OneCore-ReverseForwarders-Package.cab DISM commands and run them, as shown here:

![](bbox=[0.1, 0.25, 0.9, 0.7])

13. Let's move on to configuring ASP.NET Core. We have already copied the required DLL and XML files. Type the following commands in PowerShell:

```
$sm = Get-IISServerManager
$sm.GetApplicationHostConfiguration()
.RootSectionGroup.Sections.Add("appSettings")
$appHostconfig =
$sm.GetApplicationHostConfiguration()
$section =
```