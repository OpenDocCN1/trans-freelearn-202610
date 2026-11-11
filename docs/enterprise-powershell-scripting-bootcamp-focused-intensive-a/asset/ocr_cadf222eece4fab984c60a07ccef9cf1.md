The final section of Part 1 is to compound the extensions located by a factor of 13. This will emulate a large volume of information to pass through scripts in Part 2. You then leverage the write-host cmdlet to display "Total number of extensions: " with the number of extensions using $extensions.count. This example returns 42,713,088 extensions to be sorted.

# Part 2 - To measure the difference in duration between sorting the data using if statements and switch statements, you can perform the following:

```
$time1 = measure-command {
foreach ($extension in $extensions) {
    # Determine the file types
    if ($extension -like "*.xml") { $xmls += 1 }
    if ($extension -like "*.exe") { $exes += 1 }
    if ($extension -like "*.dll") { $dlls += 1 }
}
}
# Reset Count
$dlls = 0
$exes = 0
$xmls = 0
$time2 = measure-command {
foreach ($extension in $extensions) {
    # Determine the file types
    switch($extension) {
".dll" { $dlls += 1 }
".exe" { $exes += 1 }
".xml" { $xmls += 1 }
    default { } #do nothing
    }
}
}

$time1
$time2
$timediff = ($time1 - $time2).TotalSeconds
Write-Host "Total Number of Counted DLL files $dlls"
Write-Host "Total Number of Counted EXE files $exes"
Write-Host "Total Number of Counted XML files $xmls"
Write-Host "Total Difference in Speed: $timediff Total Seconds"
```