# # How to do it...

To start using the **IO Graphs** window with the **Advanced** feature, perform the following steps:

+   1. Start the **IO Graphs** window from the **Statistics** menu.
+   2. In the **Unit:** drop-down menu under **Y-Axis**, choose the **Advanced...** option. You will get the following window:

![](bbox=[0.122, 0.246, 0.931, 0.552])

+   3. You will see new drop-down menus with the string **SUM(*)**.
+   4. Choose **SUM(*)/COUNT FRAMES (*)/COUNT FIELDS (*)/MAX(*)/MIN(*)/AVG(*)/LOAD(*)**, and configure the appropriate filters. In the next recipes we will see some useful examples.

# ## How to monitor inter-frame time delta statistics

The time delta between frames can influence TCP performance, and there are cases in which we would like to correlate these with the performance we get from the network.

Let's look at the following capture file: