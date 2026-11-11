The preceding script displays how to create a .log file for script logging. To start, you leverage the Get-Date cmdlet with the -format parameter set to "yyyyMMddmmss" and place it into the $date variable. The $date variable will then have sequentially the four-digit year, two-digit month, two-digit day, two-digit minute, and two-digit seconds, from when the script was executed, contained in it. You then gather the computer name by leveraging the $env:COMPUTERNAME environment variable and placing the value into the $compname variable. You then combine the $compname variable with a plus symbol, an underscore for separation with a plus symbol, the date timestamp with a plus symbol, and log file name of "ServerScanScript.log", and store it in the $logname variable. You then create the full log path by specifying "c:\temp\logs\" with a plus symbol and the $logname directory and store it in the $scanlog variable. Finally, you leverage the new-item cmdlet with the -path parameter set to $scanlog, -ItemType set to File, and the -Force parameter. After execution, you will have a log file in c:\temp\logs\ with the filename of computername_datetimestamp_ServerScanScript.log.

To create a time-stamped data collection file in the format of a CSV file, you can perform the following:

```
$date = (Get-Date -format "yyyyMMddmmss")
$compname = $env:COMPUTERNAME
$logname = $compname + "_" + $date + "_ScanResults.csv"
$scanresults = "c:\temp\logs\" + $logname
new-item -path $scanresults -ItemType File -Force
```

```
# Add Content Headers to the CSV File
$csvheader = "ServerName, Classification, Other Data"
Add-Content -path $scanresults -Value $csvheader
```

The output of creating the CSV file is shown in the following screenshot:

![](bbox=[0.1, 0.65, 0.93, 0.82])