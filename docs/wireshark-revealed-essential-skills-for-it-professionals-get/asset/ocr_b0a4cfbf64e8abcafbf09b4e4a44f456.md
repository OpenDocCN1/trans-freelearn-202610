# There's more...

Now we will cover some additional helpful features.

# What is the parameter we filter?

Anytime you mark a specific field in the packet details pane, you will see the correlating filter string in the status bar at the bottom-left corner of the Wireshark window.

![](bbox=[0.07, 0.27, 0.81, 0.51])

# Adding a parameter column

You can also right-click on a parameter in the packet pane and choose Apply as Column. This will add a column with the specific parameter. For example, you can choose the parameter tcp.window_size_value and add it as a column to the packet list pane, so you will be able to watch the TCP window size online. This influences TCP performance, as we will learn in Chapter 9, UDP/TCP Analysis.

# Saving the displayed data

To save the displayed data, you can navigate to File | Export Specified Packets... and choose which packets to save.