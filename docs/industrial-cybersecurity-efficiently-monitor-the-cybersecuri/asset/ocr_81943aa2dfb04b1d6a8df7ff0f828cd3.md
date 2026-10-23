The **IPS Policy Selection** option allows us to quickly tune the rulesets to one of four predefined policies, as follows:

+   a) **Connectivity**, for preferring connectivity over security
+   b) **Balanced**, for a balanced mix of connectivity versus security
+   c) **Security**, for preferring security over connectivity
+   d) **Max-Detect**, for throwing everything but the kitchen sink at the detection engine

The following screenshot shows how this is done:

![](bbox=[0.116, 0.309, 0.848, 0.651])

Figure 9.17 – Exercise 3: Configuring IPS policy

+   11. Choose **Balanced** for now and select **Policy** for the **IPS Policy Mode** option (different than what is shown in the screen capture above) to have the rules dictate which action to take for an alert (typically, the action would be to drop the packet).
+   
+   12. Scroll down and click **Save** to finish the interface configuration.
+   
+   13. We are now ready to start Snort, so navigate to the **Services | Snort | Interfaces** page and start Snort by clicking on the **Start snort on this interface** button.