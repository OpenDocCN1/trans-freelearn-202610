22. You leverage the Write-Host cmdlet with the string "Error Accessing Path: \"$filepath\" Access Is Denied."
If this particular $err in the $errors variable matches, it will display the appropriate message on the screen.

23. After the error handling section, you continue to evaluate each $match in the $content variable.

24. You leverage the Write-Host cmdlet to display "Filename: " and the $match.FileName property.

25. You use the .Trim() method on this result to remove the leading and trailing spaces in the result. This displays the file name the match is found in.

26. You then use the Write-Host cmdlet to display "Line Number: " and the $match.LineNumber property. This displays the line number the match is found on.

27. Last, you leverage the Write-Host cmdlet to display "Line Contents: " and the $match.line property.

28. You use the .Trim() method on this result to remove the leading and trailing spaces in the result. This displays the contents of the line where the match is found.

This completes the creation of the function. After creating the function, you continue to set up the function call with the following steps:

+   1. Since you want to search for *.xml files and *.txt files, you define "*.xml","*.txt" and place it in the $include variable.
+   2. You define the $exclude variable, but set it to blank or "". This is because you don't want to exclude any items.
+   3. You then define the search word "Complete" and place the value in the $findword variable. Finally, you invoke the scan-directory function with the argument "c:\windows\system32".
+   4. After the function is invoked, scan-directory will leverage the Get-ChildItem cmdlet to determine whether the files with the $include file extension exist in the $directory.
+   5. Since the $exclude variable is blank, the script doesn't exclude any values. It is then passed through the pipeline to the select-string cmdlet to determine whether there is a -SimpleMatch that includes the $findword of "Complete".
+   6. The function reports any "access or paths exceeding 248 character" messages.
+   7. The function prints to the screen the files that meet the criteria you are searching for.