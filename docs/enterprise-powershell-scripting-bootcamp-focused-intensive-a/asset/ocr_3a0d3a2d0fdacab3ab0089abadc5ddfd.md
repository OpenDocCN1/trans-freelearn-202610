In this example, the `scan-function` cmdlet reports an access denied message on the `C:\Windows\System32\LogFiles\WMI\Rtbackup` directory. It also reports that it found the word "Complete" once in the `hppc13-pipelineconfig.xml` file and multiple times in the `Cleanup.xml` file.

You then proceed to the second set of files you want to scan for:

+   1. For the second attempt, you still search for *.xml files and *.txt files, and you define "*.xml", "*.txt" for the `$include` variable.
+   2. You define the `$exclude` variable, and set it as "*hppc13-pipelineconfig.xml*", "Cleanup.xml", to exclude both the `hppc13-pipelineconfig.xml` and `Cleanup.xml` files.
+   3. You define the search word "Complete" and place the value in the `$findword` variable.
+   4. Finally, you invoke the `scan-directory` function with the argument "C:\windows\system32\".

In the second run of the `scan-directory` function, you receive the access denied message on the `C:\Windows\System32\LogFiles\WMI\Rtbackup` directory. You don't, however, receive any messages about the `hppc13-pipelineconfig.xml` and `Cleanup.xml` files as they are defined in the exclude list. You have successfully excluded files from the search.

It is important to note that the `-Include` and `-Exclude` parameters accept arrays of objects, not comma-separated strings. If you define the exclusions as "*.xml, *.txt", the cmdlet will not return the proper results. You need to have two separate array items, separated by a comma, like "*.xml", "*.txt" for the `-Include` or `-Exclude` to work properly.

# Summary

In this chapter, you learned how to efficiently scan files and folders on a system. You started by exploring how to list folders, subfolders, and files by using the `Get-ChildItem` cmdlet. You then learned how to narrow your search by including only certain file extensions through using the `-Include` parameter. You proceeded to scan the contents of files by leveraging the `select-string` cmdlet. You then explored how to handle long file paths and access denied messages. You continued by learning how to narrow your search results by excluding specific files. Finally, you created a function to scan directories, including specific extensions, excluding specific files, and handling long file path and access denied errors.

In the next chapter, you will learn different scripting techniques to optimize your script execution speed.