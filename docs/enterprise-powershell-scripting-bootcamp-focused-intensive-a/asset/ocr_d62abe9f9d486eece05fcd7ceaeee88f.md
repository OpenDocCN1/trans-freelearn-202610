# Free Space property

Another important property is the FreeSpace property. The FreeSpace property can range from the value of the disk's Size property to 0. When scripting to determine the free space, you have to consider that the remaining space may not be the same unit of measure you started with. For example, you may have a 2-terabyte drive, but you may only have 100 kilobytes left on the disk. If you were to convert the remaining 100 kilobytes to terabytes, the value would be many decimal places long. Instead, you need to build in logic to determine the appropriate unit of measure, based on the remaining space on the drive.

One of the easiest ways to accomplish this is to create a PowerShell function that evaluates the disk space on the system. This function will allow for a $diskspace argument. It will then leverage a switch statement to evaluate the $diskspace value. If it is greater than 1PB, 1TB, 1MB, or 1KB, in that order, it will perform the appropriate math calculation to convert the appropriate unit of measure. The value, rounded to two decimal places, would then be returned to the script, along with the unit of measure. The key to success with this methodology is leveraging a function for the evaluation. When the appropriate unit of measure is found, you use the Return command and the function exits after returning the values to the script.

To convert diskspace into the respective unit of measure, you can perform the following actions:

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

measure-diskunit 868739194880123456
measure-diskunit 868739194880123
measure-diskunit 868739194880
measure-diskunit 868739194
measure-diskunit 868739
```