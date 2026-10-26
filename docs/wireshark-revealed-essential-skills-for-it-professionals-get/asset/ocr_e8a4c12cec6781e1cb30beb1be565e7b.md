# How to do it...

There are several things to notice in a network regarding STP:

+   - Which STP version is running on the network?
+   - Are there any topology changes?

# Which STP version is running on the network?

Wireshark will provide you with the version of the STP type (STP, RSTP, or MST) running on the network by looking at the **Bridge Protocol Data Units (BPDUs)**. BPDUs are the update frames that are multicast between switches.

The protocol versions are:

+   - For STP, protocol version ID equals 0
+   - For RSTP/MST, the protocol version ID equals 3

# Tip

In the standards you will not find the word "switch"; it will always be "bridge" or "multiport bridge". In this book, we will use the terms bridge and switch alternatively.

# Are there too many topology changes?

When you monitor STP operations, you may be concerned when you see many topology changes. Topology changes are normal in STP, but too many of them can have an impact on network performances.

A topology change happens when a new device is connected to the network. You can see a topology change in the following screenshot: