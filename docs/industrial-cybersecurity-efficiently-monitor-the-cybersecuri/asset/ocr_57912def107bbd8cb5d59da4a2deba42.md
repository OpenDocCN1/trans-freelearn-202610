+   12. Click on **Save**, saving the widget as `Breach Detection – Suspicious PowerShell Invocation`.

We now have a visualization around suspicious PowerShell commands and how PowerShell is invoked. Next, we are going to look at suspicious network connections.

# Suspicious egress connections

Seeing repeated connections to an external address (the Enterprise Zone or internet) can be an indication of **bot** activity on the network or exfiltration of data from your internal network (the Industrial Zone). Bots are types of malware that turn the hosts they infect into **zombie** machines, mindless drones to a master (a **command and control (C&C)** server, run by a **botmaster**), performing actions on behalf of that master. Bots need to make regular connections back to their C&C server to check in with the botmaster, asking for instructions or updates. We can detect these check-ins by monitoring for repetitive connections between an internal and an external IP address. Often, these connections are of the same type and size and are aimed at the same IP address. What we will do next is add a visualization that quickly depicts this typical type of connection in a summary table. To add a data table that summarizes outbound established connection counts, follow these instructions:

+   1. Navigate to our **Breach Detection** dashboard (**Kibana | Dashboards | Breach Detection**) and click on **Edit** in the top left of the dashboard screen.
+   2. Click on the **Create New** button to add a new widget (visualization).
+   3. Select the `Data Table` visualization type and search for and add the `Security Onion – Connections` data source.
+   4. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `source.ip` as the **Field** selection. Set the **Size** option to 50.
+   5. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `destination.ip` as the **Field** selection. Set the **Size** option to 50.
+   6. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `destination.port` as the **Field** selection. Set the **Size** option to 50.
+   7. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `network.transport.keyword` as the **Field** selection. Set the **Size** option to 10.
+   8. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `network.bytes` as the **Field** selection. Set the **Size** option to 50.
+   9. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `connection.state_description.keyword` as the **Field** selection. Set the **Size** option to 50.