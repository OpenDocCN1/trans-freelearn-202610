The way PowerShell is started can give away any malicious intent or intended abuse as well. The following table summarizes some suspicious ways PowerShell can be invoked (started):

| Parameter | Variations | Purpose of parameter |
|---|---|---|
| -noprofile | -nop | Skips loading of profile.ps1 and avoids logging |
| -encoded | -e -en -enc | Lets a user run encoded PowerShell scripts |
| -ExecutionPolicy bypass | -ep bypass, -exp bypass, -exec bypass | Bypasses any execution policy |
| -windowStyle hidden |  | Prevents the creation of a window; may generate false positives |
| -version 2 | -v 2, -version 2.0 | Forces use of PowerShell version 2 |

Detecting suspicious invocation of the PowerShell engine can show malicious intent. We will add a data table that lists any process creation logs that contain these suspicious command-line parameters. The logs this data is in are provided by the Sysmon logging engine that we deployed earlier, in Exercise 1 – Using Wazuh to add Sysmon logging. Follow these steps to get the data table added to our dashboard:

+   1. Navigate to our **Breach Detection** dashboard (**Kibana | Dashboards | Breach Detection**) and click on **Edit** in the top left of the dashboard screen.
+   2. Click on the **Create New** button to add a new widget (visualization).
+   3. Select the **Data Table** visualization type and search for and add the `Security Onion Sysmon` data source.
+   4. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `process.command_line.keyword` as the **Field** selection.
+   5. Set the **Size** option to 50 and click **Update**.
+   6. We will be adding the source computer name to show this with the PowerShell invocation event entries. To do this, add an additional `Split Rows` data bucket and set **Aggregation** to **Terms**, with `winlog.computer.keyword` as the **Field** selection.
+   7. Set the **Size** option to 50 and click **Update**.