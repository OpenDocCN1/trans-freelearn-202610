Here is some information to keep in mind when exploiting this feature:

+   - Scripts run as the System account on Windows VMs and as an elevated user (typically root) on Linux VMs.
+   - We can only run one script at a time through the feature.
+   - Interactive scripts that prompt for user information are not supported.
+   - Outbound connectivity from the VM to Azure public IP addresses on port 443 is required to return the results of the script.

The easiest way to get started with the Run Command functionality is from the **Virtual machine** blade in the portal (*Figure 5.12*). There are some prebuilt commands that can be run in the portal but for our use case, we will want to run our own custom commands:

![](bbox=[0.12, 0.365, 0.85, 0.705])

Figure 5.12 – Azure Run command menu