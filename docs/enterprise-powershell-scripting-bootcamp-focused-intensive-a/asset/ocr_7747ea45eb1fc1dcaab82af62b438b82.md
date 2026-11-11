+   - **Kill file location:** This will provide the location of where the script termination file is located.
+   - **Directory scan list:** This will list the directories you want to scan for sensitive data. This will also include the file types that you want to scan.
+   - **File ignore list:** This will list the files that you want the script to ignore during the file scan portion of the script.
+   - **Search ignore list:** This will be the negative search terms, or false positives, that you want the script to ignore.
+   - **Built-in users list:** This is the list of the built-in users and accounts to be ignored while scanning a system.

> The `Scan_Answers.xml` answer file used in this chapter can be found in the code files for this chapter. This will be required to execute the examples in this chapter.

The `answer` file for a Windows server scanning script is designed to be a one-time use file. This is to prevent sensitive data sitting on a remote system, which could eventually be compromised. Additionally, since the `answer` file has an AFD, it makes brute forcing the encoding and encryption exponentially more difficult. The best time to delete the `answer` file is directly after placing contents into memory. PowerShell will only need to interact with the in-memory version of the `answer` file after the initial read, making it the perfect time to delete the file.

# PowerShell script 3 - the termination files

There may come a time where you will need to terminate the execution of the Windows server scanning script. This could be due to the script consuming too many resources, or the script taking too long to execute. One of the most efficient ways to quickly and gracefully terminate a script is leveraging a `termination` file. When a `termination` file is placed on the system, the script will stop its current action, and gracefully exit the script.

The two methods to deploy the `termination` file in an environment are either locally on the server, or by leveraging remote commands. When you leverage deployment tools, you may have to create a script that runs locally on the system. If you want to create the `termination` file locally on a system, you can leverage the `new-item` cmdlet with the `-path` parameter set to "c:\temp\KILL_SERVER_SCAN.NOW". You then specify the `-ItemType` parameter with the argument of `File`. After execution, the `termination` file will be created.