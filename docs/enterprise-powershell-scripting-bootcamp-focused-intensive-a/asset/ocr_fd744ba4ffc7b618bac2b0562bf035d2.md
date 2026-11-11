# Improving Performance by Using Regular Expressions

```
$searchRegex = '^.*(' + (($types | foreach {[regex]::escape($_)}) -join "|") + ').*$'

$time2 = Measure-Command {
    foreach ($file in $files) {
        if ($file -match $searchRegex) { $filefound += 1 }
    }
}

Write-host "Total Number of Found Files $filefound"
$time1
$time2

$timediff = ($time1 - $time2).TotalSeconds
Write-host "Total Difference in Speed: $timediff Total Seconds"
```

The output of this is shown in the following screenshot:

![](bbox=[0.16, 0.38, 0.9, 0.9])