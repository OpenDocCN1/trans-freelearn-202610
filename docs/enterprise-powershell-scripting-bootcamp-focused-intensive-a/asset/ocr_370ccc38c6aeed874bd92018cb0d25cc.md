In this example, you create a function to measure disk units of measure and scan the individual disk configuration on a system:

+   1. You first start by defining the measure-diskunit function with the parameter block of param($diskspace).
+   2. You then create a switch statement evaluating the $diskspace variable.
+   3. The script evaluates for petabytes, terabytes, gigabytes, megabytes, and kilobytes listed in descending order.
+   4. If the script returns True for a unit of measure, it will divide the value by the unit measure and round the value to the tenths place.
+   5. It will return an array with both the converted disk space and unit of measure label. You then define the scan-disks function to evaluate the disks on a system.
+   6. You call the check-kill function to determine if the script should continue or terminate.
+   7. If the script can continue, you leverage the win32_logicaldisk class to determine each disk's drive letter, free space, and disk size.
+   8. You leverage the measure-diskunit function to determine the unit of measure and the value in that unit of measure.
+   9. You then leverage a switch statement to evaluate the drive type number.
+   10. The corresponding number will return a string value for the type of disk.
+   11. You complete the scan-disks function by leveraging the log function with "DiskConfiguration, Drive $driveletter | Drive Type: $type | Size: $convsize $sunit | Freespace: $convFreeSpc $funit".

# Scheduled tasks function

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