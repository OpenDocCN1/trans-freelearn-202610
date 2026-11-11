
- 3. You then leverage the log function with the "Kill File Detected at $date. Terminating Script." "Y" "Y" value.
- 4. This records the kill file termination to the event log and log file, if enabled.
- 5. You then leverage the log function again with the "KillFile, Kill File Detected at $date. Terminating Script" value.
- 6. This logs the kill file detection in the CSV file.
- 7. You then leverage the copy-item cmdlet to copy the $scnresults file to the $csvunc.id path with the -Force parameter.
- 8. You also leverage the copy-item cmdlet to copy the $scanlog file to the $csvunc.id path with the -Force parameter.
- 9. Finally, you will need to leverage the exit command to exit the script.

After defining the function, you call the function with check-kill. When the function evaluates, the file does not exist, so it allows the script to continue to the next operation. If the file exists, it will terminate and copy the scan results and scan log to the UNC/file path provided.

## Multiple Windows server scanning script functions

After you finish creating the entire core scripting components, you start defining the individual scanning functions for the script. The scanning functions will evaluate multiple aspects of the system and record all the results in the CSV file. You will create functions for scanning disks, scheduled tasks, processes, Windows services, software, user profiles, Windows features, and directories for files containing strings. You will also create a measure-diskunit function for converting disk storage units.

To create the measure-diskunit and scan-disk functions, you can perform the following:

```
function measure-diskunit { param($diskspace)
    switch ($diskspace) {
        {$_ -gt 1PB} { return [System.Math]::Round(($_ / 1PB),2),"PB" }
        {$_ -gt 1TB} { return [System.Math]::Round(($_ / 1TB),2),"TB" }
        {$_ -gt 1GB} { return [System.Math]::Round(($_ / 1GB),2),"GB" }
        {$_ -gt 1MB} { return [System.Math]::Round(($_ / 1MB),2),"MB" }
        {$_ -gt 1KB} { return [System.Math]::Round(($_ / 1KB),2),"KB" }
        default { return [System.Math]::Round(($_ / 1MB),2),"MB" }
    }
}
```

```
function scan-disks {
    check-kill
    $disks = get-wmiobject win32_logicaldisk
    Foreach ($disk in $disks) {
        $driveletter = $disk.DeviceID
        $freespace = $disk.FreeSpace
        $size = $disk.Size

        if ($freespace -lt 1) { $freespace = "0" }
        if ($size -lt 1) { $size = "0" }

        $freetype = measure-diskunit $freespace
        $convFreeSpc = $freetype[0]
        $funit = $freetype[1]

        $sizetype = measure-diskunit $size
        $convsize = $sizetype[0]
        $sunit = $sizetype[1]

        switch ($disk.DriveType) {
            0 { $type = "Type Unknown." }
            1 { $type = "Doesn't have a Root Directory." }
            2 { $type = "Removable Disk" }
            3 { $type = "Local Disk" }
            4 { $type = "Network Drive" }
            5 { $type = "Compact Disk" }
            6 { $type = "RAM Disk" }
            default { $type = "Unable To Determine Drive Type!" }
        }
        log "DiskConfiguration, Drive $driveletter | Drive Type: $type | Size: $convsize $sunit  | Freespace: $convFreeSpc $funit"
    }
}
```

There is no output to display from creating this function.

In this example, you create a function to measure disk units of measure and scan the individual disk configuration on a system:

- 1. You first start by defining the measure-diskunit function with the parameter block of param($diskspace).
- 2. You then create a switch statement evaluating the $diskspace variable.
- 3. The script evaluates for petabytes, terabytes, gigabytes, megabytes, and kilobytes listed in descending order.
- 4. If the script returns True for a unit of measure, it will divide the value by the unit measure and round the value to the tenths place.
- 5. It will return an array with both the converted disk space and unit of measure label. You then define the scan-disks function to evaluate the disks on a system.
- 6. You call the check-kill function to determine if the script should continue or terminate.
- 7. If the script can continue, you leverage the win32_logicaldisk class to determine each disk's drive letter, free space, and disk size.
- 8. You leverage the measure-diskunit function to determine the unit of measure and the value in that unit of measure.
- 9. You then leverage a switch statement to evaluate the drive type number.
- 10. The corresponding number will return a string value for the type of disk.
- 11. You complete the scan-disks function by leveraging the log function with "DiskConfiguration, Drive $driveletter | Drive Type: $type | Size: $convsize $sunit | Freespace: $convFreeSpc $funit".

## Scheduled tasks function

To create a function to scan scheduled tasks that are not running as built-in users, you can perform the following:

```
function scan-schtasks {
    check-kill
    $schtasks = Get-ScheduledTask
    foreach ($Task in $schtasks) {
        $tskUser = $Task.Principal.UserId
        if($tskUser -eq $null) { $tskuser = "SYSTEM" }
        if (!( $tskUser -match $blstRegEx) ) {
            $tskname = ($task.TaskName).replace(","," ")
        }
    }
}
```

```
$tskpath = $task.TaskPath
log "ScheduledTsksData, Scheduled task with the name of $tskname in the location of $tskpath is running as $tskuser"
}
}
}
```

There is no output to display from creating this function.

In this example, you create a function to scan scheduled tasks that are not running as built-in users:

You start by defining the scan-schtasks function.

You call the check-kill function to determine if the script should continue or terminate.

If the script can continue, you leverage the Get-ScheduledTask cmdlet to gather all scheduled tasks.

You evaluate each scheduled task and determine if the Principal.UserID property is matched to the built-in user regular expression.

If the user is not matched by the $blstRegEx regular expression or, !($tskuser -match $blstRegEx), you gather the scheduled task name and scheduled task path.

You complete the function by leveraging the log function with the "ScheduledTsksData, Scheduled task with the name of $tskname in the location of $tskpath is running as $tskuser" value.

## Windows processes function

To create a function to scan the processes on the system for users that are not built-in, you can perform the following:

```
function scan-process {
    check-kill
    $processes = Get-WmiObject win32_process
    foreach ($process in $processes) {
        $procuser = $process.GetOwner().User
        if (!( $procuser -match $blstRegEx)) {
            $procname = $process.Name
            $procdom = $process.GetOwner().Domain
```

```
log "WindowsProcessData, $procname is running with the $procdom\$procuser account."
    }
}
}
```

There is no output to display from creating this function.

In this example, you create a function to scan processes that are not running as built-in users:

You start by defining the scan-process function. You call the check-kill function to determine if the script should continue or terminate.

If the script can continue, you leverage the win32_process WMI class to obtain the current running processes.

You continue to evaluate each process to determine if the .GetOwner().User property is matched to the built-in user regular expression.

If the user is not matched by the $blstRegex regular expression or, !($procuser -match $blstRegEx), you gather the process name, process user's domain, and process username.

You complete the function by leveraging the log function with the "WindowsProcessData, $procname is running with the $procdom\$procuser account." value.

## Windows services function

To create a function to scan services on the system for users that are not built-in, you can perform the following:

```
function scan-services {
    check-kill
    $service = get-wmiobject win32_service
    foreach ($service in $services) {
        $svcAuthUser = $service.StartName
        if (!($svcAuthUser -match $blstRegEx)) {
            $svcdisplay = $service.DisplayName
            log "WindowsServicedata, Service with $svcdisplay name is running with $svcAuthUser account."
```
