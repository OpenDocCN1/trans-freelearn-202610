+   23. If this evaluates to true, the script will take the $err.Exception property and convert the Exception to a string.
+   24. You evaluate the string to split it by the apostrophe character.
+   25. You specify [1] to select the second value, which is the Filename, and set it to the $filepath variable.
+   26. You leverage the Write-Host cmdlet with the string "Error Accessing Path: "$filepath" Access Is Denied.".
+   27. If this particular $err in the $errors variable matches, it will display the appropriate message on the screen.
+   The final step is the cleanup portion of the script.
+   28. To clean up the script, you execute cd c: to change the directory back to the c: drive.
+   29. You disconnect the H drive by leveraging the Remove-PSDrive cmdlet with the -name parameter set to H and the -Force parameter.
+   30. You close the SMB share, leveraging the Remove-SmbShare cmdlet with the -name parameter set to "UserData" and the -Force parameter.

If this script was executed on a server file share, the Get-ChildItem cmdlet would report all of the files and directories in the $directory variable. It would also print out to screen if there were any directories longer than 248 characters, and if there were any access denied error messages generated on the folder structure.

# Excluding files

As you perform scanning in your environment, you will eventually want to filter out some of the false positives. This is especially true if you leverage a template to build your systems, as a multitude of commonalities might be detected. You may also want to filter out very large files that are known to be safe to optimize the scanning duration.

The Get-ChildItem cmdlet includes the -exclude parameter, which enables you to specify a string of objects that you want to exclude. The -exclude parameter also supports multiple values separated by commas and wildcards with the asterisk character. To start, you specify the Get-ChildItem cmdlet with a directory to scan as the argument. You then provide the -exclude parameter with a file specified as its argument. When the Get-ChildItem cmdlet executes, it will filter out those items specified as the arguments to the -exclude parameter.