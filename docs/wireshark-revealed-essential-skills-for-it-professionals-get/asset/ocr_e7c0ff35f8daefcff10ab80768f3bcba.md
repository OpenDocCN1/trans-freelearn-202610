# # How to do it...

To try and find out what a problem could be, connect your laptop with the Wireshark to the network, and port mirror the suspected clients or server as described below. In the following sections, we will see several scenarios for several problems.

There are many predefined filters that are used with NetBIOS. You can find them by clicking on the **Expression** button, which is on the right-hand side of the **Display Filters** window.

+   1. For general NetBIOS commands, they start with `netbios`.
+   2. For NetBIOS name service, they start with `nbns`.
+   3. For NetBIOS datagram service, they start with `nbds`.
+   4. For NetBIOS session service, they start with `nbss`.
+   5. For SMB, they start with `smb`.

# ## General tests

First, take a general look at the network. Then, look for suspicious patterns:

+   1. Connect Wireshark to the network. Each one of the ports will do fine, as long as you are on the same broadcast domain with the clients that are having the problems.
+   2. Configure the display filter `nbns.flags.response == 0`. It will give you the NBNS requests. You will see many broadcasts, as shown in the following screenshot:

![](bbox=[0.123, 0.642, 0.926, 0.844])

+   3. As you saw in the previous screenshot, in the capture file you will see the following:
+      - **NBNS registration packets (1):** In the examples, there are registrations with