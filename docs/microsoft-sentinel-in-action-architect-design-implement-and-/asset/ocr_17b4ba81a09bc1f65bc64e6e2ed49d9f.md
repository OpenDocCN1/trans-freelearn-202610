We can see in the **Relevant analytic templates** section there are currently 16 templates available. Each template needs to be enabled to ensure the data is being analyzed. Select the first analytics template, and then walk through the configuration steps, as shown in the following screenshot:

![](bbox=[0.15, 0.19, 0.85, 0.68])

Figure 3.9 – AWS analytics rule for Known IRIDIUM IP

Configure each page of the wizard with your desired configuration requirements, then finish the wizard and watch for the resulting impact in the number of incidents raised. If the volume is too high, reconfigure the rule to reduce how often an alert is generated, or enable the ability to group alerts into a single instance. Be careful not to over-tune the alerts to the degree where no incidents are created, as this could hide legitimate alerts that require investigation.

In the next section, we will configure the storage options available for Log Analytics.