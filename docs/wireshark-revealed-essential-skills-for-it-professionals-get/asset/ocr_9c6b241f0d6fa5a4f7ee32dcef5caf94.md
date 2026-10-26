# How to do it...

For locating a problem when MS-TS is involved, start with going to the users and asking questions. Follow these steps:

+   1. When users complain about a slow network, ask them a simple question: Do they see the slowness in the data presented on the screen, or when they switch between windows?
+   2. If they say that the switch between windows is very fast, it is not an MS-TS problem. MS-TS problems will cause slow window changes, picture freezes, slow scrolling of graphical documents, and so on.
+   3. If they say that they are trying to generate a report (when the software is running over MS-TS) but the report is generated after a long period of time, this is a database problem and not MS-TS or Citrix.
+   4. When a user works with MS-TS over a high-delay communication line and types very quickly, they might experience delays with the characters. This is because MS-TS is transferring window changes, and with high delays these windows changes will be transferred slowly.
+   5. When measuring the communication line with Wireshark:
+      - Use IO graphs for monitoring the line
+      - Use filters to monitor the upstream and the downstream directions
+      - Configure bits per second on the y-axis
+   6. You will get the following screenshot: