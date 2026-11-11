An alternative to killing the PowerShell processes is to create a function to check to see if a kill file exists on the system. If that kill file exists on the system, the script will gracefully terminate. This will allow the script to stop the current process, copy the partially created scanned data to a UNC path, and exit the PowerShell script. This will provide a much more robust script termination experience. You insert the function throughout your Windows server scanning script to check the file's existence. The general rule of thumb is to check for the existence of the termination file as the first line of each function. You will also want to check for the file in-between iterations of long scanning operations, such as scanning files on a system. After each file scan, you should run the file check. While this may slightly slow down your script, it provides quick termination of scripts that are in process.

To create a termination function, you start by defining a function named check-kill. You then leverage the test-path cmdlet to verify that the kill file doesn't exist on the system. The kill file should be stored in a common area, such as c:\temp\, with a unique name, such as KILL_SERVER_SCAN.NOW. If test-path returns True, you generate a date timestamp, record that the kill file was detected, and copy the data-collected log files and data to its destination. You then exit the script.

To create a function to check if a kill file exists, report that the file is detected, and then exit the script. You can perform this as follows:

```
```powershell
function check-kill {
    if (test-path $killfile.id) {
        $date = (Get-Date -format "yyyyMMddmmss")
        log "Kill File Detected at $date. Terminating Script." "y" "y"
        log "KillFile, Kill File Detected at $date.. Terminating Script"
        copy-item -Path $scnresults -Destination $csvunc.id -Force
        copy-Item -Path $scanlog -Destination $csvunc.id -Force
        exit
    }
}
```
```

There is no output from this script as it exits the PowerShell instance.

In this example, you create a function to detect if a kill file exists, report that the file is detected, copy the files to the $csvunc, and exit the script:

+   1. You start by declaring the check-file function. You create an if statement with the test-path cmdlet to determine if the $killfile.id exists.
+   2. If it does, you obtain a date timestamp leveraging the Get-Date cmdlet with the -format parameter set to the "yyyyMMddmmss" argument.