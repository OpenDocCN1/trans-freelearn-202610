```
# if err is not set, it was successful.
if (!$err) {
    Write-host "Succesfully loaded the .NET System.Security Assembly For Encryption"
}
```

The output from this command looks like the following:

![](bbox=[0.06, 0.21, 0.89, 0.36])

In this example, you successfully import the .NET System.Security Assembly for use with PowerShell. You first start by writing "Loading the .NET System.Security Assembly for Encryption" to the screen using the Write-host command. You then leverage the Add-Type cmdlet with the -AssemblyName parameter with the System.Security argument, the -ErrorAction parameter with the SilentlyContinue argument, and the -ErrorVariable parameter with the err argument. You then create an if statement to see if $err contains data. If it does, it will use Write-host cmdlet to print "Error Importing the .NET System.Security Assembly." to the screen. It will PAUSE the script so the error can be read. Finally, it will exit the script. If $err is $null, designated by if (!$err) {, it will use the Write-host cmdlet to print "Successfully loaded the .NET System.Security Assembly for Encryption" to the screen. At this point, the script or PowerShell window is ready to leverage System.Security Assembly.

![](bbox=[0.12, 0.66, 0.82, 0.86])