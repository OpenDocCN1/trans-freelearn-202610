different TCP streams.

![](bbox=[0.072, 0.086, 0.928, 0.31])

In the graph of the preceding screenshot, you can see that retransmissions from each TCP stream are presented in different colors.

# How to monitor a number of field appearances

In various network protocols (mostly on those running over TCP), variations in time between frames (that is, the frame-time delta filter) can influence the performance significantly. One of the tools for viewing these changes in the IO Graphs window is the Advanced... configuration.

To do it, perform the following steps:

+   1. Right-click on a packet in the suspicious TCP stream and navigate to Conversation filter | TCP. A filter will appear in the main filter box.
+   2. Open IO Graph from the Statistics menu.
+   3. Under Y-Axis, choose Advanced... for Unit:.
+   4. Configure the filters as follows:
+      - Copy the filter definition from the upper filter box on the right-hand side to the IO Graph filter box on the left-hand side
+      - On the left-hand side, type the filter frame.time_delta
+      - Choose AVG(*) to see the average delta.
+   5. Choose the appropriate X-Axis resolution.

Here is an example. In the following screenshot, we see a packet list with time