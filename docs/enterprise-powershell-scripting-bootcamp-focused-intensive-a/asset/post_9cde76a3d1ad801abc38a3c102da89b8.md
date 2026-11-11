
To create a function to scan a system for the word "Complete" in .xml and .txt files without exclusions, then again with exclusions, you can perform the following actions:

```
function scan-directory { param($directory)
    $errors = @()
    $content = get-childitem $directory -Include $include -Exclude $exclude -recurse -ErrorActionSilentlyContinue -ErrorVariable +errors | select-string -Pattern $findword -SimpleMatch -ErrorActionSilentlyContinue
    if ($errors) {
        foreach ($err in $errors) {
            if ($err.Exception -like "*Could not find a part of the path*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ may be over 248 Characters."
            }
            if ($err.Exception -like "*is denied.*") {
                $filepath = ($err.Exception).ToString().split(" ")[1]
                Write-Host "Error Accessing Path: ~"$filepath"~ Access Is Denied."
            }
        }
    }
    foreach ($match in $content) {
        Write-Host "Filename: " ($match.FileName).Trim()
        Write-Host "Line Number: " $match.LineNumber
        Write-Host "Line Contents: " ($match.Line).Trim()
    }
}
```

$include = "*.xml","*.txt"
$exclude = ""
$findword = "Complete"
scan-directory "c:\Windows\System32\"

$include = "*.xml","*.txt"

```
$exclude = "*hppc13-pipelineconfig.xml*", "Cleanup.xml"
$findword = "Complete"
scan-directory "c:\Windows\System32\"
```

The output of this is shown in the following screenshot:

![](img/a27a138d6586c10902d8e86ab3384834_146_0.png)

This example displays how to scan a directory structure for .xml and .txt files for the word "Complete". It also displays how to successfully exclude results returned from the scanning function:

- 1. You first start by creating a function named `scan-directory`.
- 2. You create a parameter block setting the first argument as `$directory`.
- 3. To start the function you declare the `$errors` array by setting `$errors` equal to `@()`.
- 4. You call the `Get-ChildItem` cmdlet with the `$directory` argument, the `-Include` parameter with the `$include` argument, and the `-exclude` parameter with the `$exclude` argument.
- 5. You continue to include the `-recurse` parameter for subdirectory scanning, the `-ErrorAction` parameter set to the `SilentlyContinue` argument, and the `-ErrorVariable` set to the `+errors` additive operator.
- 6. The `Get-ChildItem` cmdlet statement is then piped to the `select-string` cmdlet with the `$findword` argument, the `-SimpleMatch` parameter, and the `-ErrorAction` of `SilentlyContinue`.
- 7. This section will query the $directory location, and its subdirectories, for the files types specified in $include, excluding the files in $exclude, and select the strings that has -Simplematch $findword.
- 8. The -ErrorAction is applied to the select-string cmdlet to suppress the errors on files and folders that cannot be accessed, or are greater than 248 characters.
- 9. Those errors are already added to the $errors variable via the Get-ChildItem cmdlet.
- 10. The result from this command is then stored in the $content variable.
- 11. After the Get-ChildItem cmdlet completes, you evaluate the $errors variable to determine whether there are any values in it.
- 12. Since you created a folder structure longer than 248 characters, the $errors variable will have data in it, causing the if statement to evaluate to $true. The script then continues to perform a foreach loop to evaluate each $err in the $errors array.
- 13. The script then performs an if statement to determine whether the $err.Exception property is like "*Could not find a part of the path*".
- 14. If this evaluates to true, the script will take the $err.Exception property and convert the Exception to a string.
- 15. You evaluate the string to split it by the apostrophe character.
- 16. You specify [1] to select the second value, which is the filename, and set it to the $filepath variable.
- 17. You leverage the Write-Host cmdlet with the string "Error Accessing Path:""$filepath"" may be over 248 Characters". If this particular $err in the $errors variable matches, it will display the appropriate message on the screen.
- You proceed to your second evaluation of the $errors variable.
- 18. During this evaluation, you determine if the $err.Exception property is like "*is denied.*". This catches access denied error messages that may be occurring on the system.
- 19. If this evaluates to true, the script will take the $err.Exception property and convert the Exception to a string.
- 20. You then evaluate the string to split it by the apostrophe character.
- 21. You specify [1] to select the second value, which is the filename, and set it to the $filepath variable.
- 22. You leverage the Write-Host cmdlet with the string "Error Accessing Path: \"$filepath\" Access Is Denied." If this particular $err in the $errors variable matches, it will display the appropriate message on the screen.
- 23. After the error handling section, you continue to evaluate each $match in the $content variable.
- 24. You leverage the Write-Host cmdlet to display "Filename: " and the $match.FileName property.
- 25. You use the .Trim() method on this result to remove the leading and trailing spaces in the result. This displays the file name the match is found in.
- 26. You then use the Write-Host cmdlet to display "Line Number: " and the $match.LineNumber property. This displays the line number the match is found on.
- 27. Last, you leverage the Write-Host cmdlet to display "Line Contents: " and the $match.line property.
- 28. You use the .Trim() method on this result to remove the leading and trailing spaces in the result. This displays the contents of the line where the match is found.

This completes the creation of the function. After creating the function, you continue to set up the function call with the following steps:

- 1. Since you want to search for *.xml files and *.txt files, you define "*.xml","*.txt" and place it in the $include variable.
- 2. You define the $exclude variable, but set it to blank or "". This is because you don't want to exclude any items.
- 3. You then define the search word "Complete" and place the value in the $findword variable. Finally, you invoke the scan-directory function with the argument "c:\windows\system32".
- 4. After the function is invoked, scan-directory will leverage the Get-ChildItem cmdlet to determine whether the files with the $include file extension exist in the $directory.
- 5. Since the $exclude variable is blank, the script doesn't exclude any values. It is then passed through the pipeline to the select-string cmdlet to determine whether there is a -SimpleMatch that includes the $findword of "Complete".
- 6. The function reports any "access or paths exceeding 248 character" messages.
- 7. The function prints to the screen the files that meet the criteria you are searching for.

In this example, the `scan-directory` cmdlet reports an access denied message on the `C:\Windows\System32\LogFiles\WMI\Rtbackup` directory. It also reports that it found the word "Complete" once in the `hppc13-pipelineconfig.xml` file and multiple times in the `Cleanup.xml` file.

You then proceed to the second set of files you want to scan for:

- 1. For the second attempt, you still search for *.xml files and *.txt files, and you define "*.xml", "*.txt" for the `$include` variable.
- 2. You define the `$exclude` variable, and set it as "*hppc13-pipelineconfig.xml*", "Cleanup.xml", to exclude both the `hppc13-pipelineconfig.xml` and `Cleanup.xml` files.
- 3. You define the search word "Complete" and place the value in the `$findword` variable.
- 4. Finally, you invoke the `scan-directory` function with the argument "C:\windows\system32\".

In the second run of the `scan-directory` function, you receive the access denied message on the `C:\Windows\System32\LogFiles\WMI\Rtbackup` directory. You don't, however, receive any messages about the `hppc13-pipelineconfig.xml` and `Cleanup.xml` files as they are defined in the exclude list. You have successfully excluded files from the search.

It is important to note that the `-Include` and `-Exclude` parameters accept arrays of objects, not comma-separated strings. If you define the exclusions as "*.xml, *.txt", the cmdlet will not return the proper results. You need to have two separate array items, separated by a comma, like "*.xml", "*.txt" for the `-Include` or `-Exclude` to work properly.

## Summary

In this chapter, you learned how to efficiently scan files and folders on a system. You started by exploring how to list folders, subfolders, and files by using the `Get-ChildItem` cmdlet. You then learned how to narrow your search by including only certain file extensions through using the `-Include` parameter. You proceeded to scan the contents of files by leveraging the `select-string` cmdlet. You then explored how to handle long file paths and access denied messages. You continued by learning how to narrow your search results by excluding specific files. Finally, you created a function to scan directories, including specific extensions, excluding specific files, and handling long file path and access denied errors.

In the next chapter, you will learn different scripting techniques to optimize your script execution speed.
