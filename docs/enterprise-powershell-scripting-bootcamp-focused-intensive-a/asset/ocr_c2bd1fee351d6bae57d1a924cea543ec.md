The output of this is shown in the following screenshot:

![](bbox=[0.13, 0.1, 0.81, 0.19])

In this example, you timestamp the start of the execution of the script:

You start by leveraging the Get-Date cmdlet with the -format parameter set to the "yyyyMMddhhmmss" argument.

You then leverage the log function with "Starting Windows Server Scanning Script at $date ..." as the first parameter, "Y" as the second parameter, and "Y" as the third parameter.

The output is "Starting Windows Server Scanning Script at 201703201039...".

You then call the log function with "ScriptStart,$date" as the first parameter. The output is ScriptStart,201703201039.

# Creating a termination function

When PowerShell scripts execute on systems, they consume memory and CPU resources. This is especially true if you are scanning directories and leveraging the Get-Content cmdlet to evaluate files. PowerShell has to load the directory listing in memory, and place the entire contents of the file in memory for processing.

One of the common concerns with deploying a PowerShell script enterprise-wide is being able to terminate the script mid-scan if necessary. While you could use the stop-process cmdlet or the taskkill.exe to terminate the PowerShell processes, you may also inadvertently terminate all running PowerShell processes on a system. Without fully executing the script, you may be leaving sensitive scan data on the system.