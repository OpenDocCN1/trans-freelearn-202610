+   4. Since the write-progress cmdlet doesn't auto-round the percent complete, you have to leverage the [math].Net assembly to truncate the percent complete to a whole number.
+   5. To calculate the percent complete, you take the current attempt, divided by the total number of iterations (10,000), and obtain the percentage by multiplying the value by 100. This rounded value is then stored in the $perComplete variable.
+   6. You continue to use the write-progress cmdlet with the -Activity parameter set to "Looping Retrieval of Contents of c:\Windows:", the -status parameter set to "$attempt of 10000.", and the -PercentComplete parameter set to $perComplete.
+   7. Finally, you call the Get-ChildItem cmdlet with the c:\windows\ argument. This action will repeat 10,000 times.
+   The execution duration will be captured by the measure-command cmdlet and stored in the $time1 variable.
+   8. You then set up a second measuring command.
+   9. You declare the measure-command cmdlet.
+   10. You declare the range from 1 to 10000 and pipe that to the %.
+   11. You leverage the Get-ChildItem cmdlet with the "c:\Windows\" argument and set those results to $contents.
+   12. This action will repeat 10,000 times. The execution duration will be captured by the measure-command cmdlet and stored in the $time2 variable.
+   After execution of the second command, you display the duration in $time1 and $time2 by calling the variables.
+   13. You then calculate the difference in execution time by calling $time1 - $time2 and referencing the TotalSeconds property.
+   14. You set this value in the $timediff variable.
+   15. Finally, you print the time value to the display by using the write-host cmdlet with "Total Difference in Speed: $timediff Total Seconds".

You will see that, in this example, keeping operations in memory is about 219% times as fast than using the write-progress cmdlet.