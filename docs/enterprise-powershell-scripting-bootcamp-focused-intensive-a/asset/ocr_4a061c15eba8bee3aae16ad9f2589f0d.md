+   5. The files that have the appropriate extensions, and contain strings that match $srtermsRegEx will be placed in the $content variable.
+   6. In the event that the Get-ChildItem cmdlet has errors obtaining content, you catch each of the errors and store them in the $errors array.
+   7. You then process each error by looking at the Exception property.
+   8. If the exception is -like *Could not find a part of the path*, the script will leverage the log function with the "FileScanData, Error Accessing Path: "$filepath" may be over 248 Characters." value.
+   9. If the exception is -like *is denied.*, the script will leverage the log function with the "FileScanData, Error Accessing Path: "$filepath" Access Is Denied." value. If no errors are found, the script will evaluate each of the filenames that were found.
+   10. If the user is not matched by the $flingRegEx regular expression or, !($filename -match $flingRegEx), the script will obtain the match line number and contents of the match line.
+   11. Finally, you leverage the log function with the "FileScanData, String match found in file named $filename with the line number of $lineno and the line contents of $linecontents." value.

# Invoking the functions

To check if the functions are enabled and to invoke the functions if they are, you can perform the following:

```
if ($scanDisks.id -eq "True") { scan-disks }
if ($scanSchTasks.id -eq "True") { scan-schtasks }
if ($scanProcess.id -eq "True") { scan-process }
if ($scanServices.id -eq "True") { scan-services }
if ($scanSoftware.id -eq "True") { scan-software }
if ($scanProfiles.id -eq "True") { scan-profiles }
if ($scanFeatures.id -eq "True") { scan-features }
if ($scanFiles.id -eq "True") { scan-directory }
```

The output of this function will vary if you have verbose logging enabled.

In this example, you check to see if the Windows server scanning functions are enabled and invoke the functions if they are. For each of the features, you query the .ID attribute of the XML file answers to determine if the value is set to True or False. If the value is True, the respective function will execute.