+   7. This section will query the $directory location, and its subdirectories, for the files types specified in $include, excluding the files in $exclude, and select the strings that has -Simplematch $findword.
+   8. The -ErrorAction is applied to the select-string cmdlet to suppress the errors on files and folders that cannot be accessed, or are greater than 248 characters.
+   9. Those errors are already added to the $errors variable via the Get-ChildItem cmdlet.
+   10. The result from this command is then stored in the $content variable.
+   11. After the Get-ChildItem cmdlet completes, you evaluate the $errors variable to determine whether there are any values in it.
+   12. Since you created a folder structure longer than 248 characters, the $errors variable will have data in it, causing the if statement to evaluate to $true. The script then continues to perform a foreach loop to evaluate each $err in the $errors array.
+   13. The script then performs an if statement to determine whether the $err.Exception property is like "*Could not find a part of the path*".
+   14. If this evaluates to true, the script will take the $err.Exception property and convert the Exception to a string.
+   15. You evaluate the string to split it by the apostrophe character.
+   16. You specify [1] to select the second value, which is the filename, and set it to the $filepath variable.
+   17. You leverage the Write-Host cmdlet with the string "Error Accessing Path:""$filepath"" may be over 248 Characters". If this particular $err in the $errors variable matches, it will display the appropriate message on the screen.
+   You proceed to your second evaluation of the $errors variable.
+   18. During this evaluation, you determine if the $err.Exception property is like "*is denied.*". This catches access denied error messages that may be occurring on the system.
+   19. If this evaluates to true, the script will take the $err.Exception property and convert the Exception to a string.
+   20. You then evaluate the string to split it by the apostrophe character.
+   21. You specify [1] to select the second value, which is the filename, and set it to the $filepath variable.