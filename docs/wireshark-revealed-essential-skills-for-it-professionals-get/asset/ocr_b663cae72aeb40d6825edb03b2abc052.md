# # How it works...

Tags are small pieces of data added to a packet in order to add VLAN information to it. The tag is a 4-byte long string (32 bits), as presented in one of the following diagrams.

Most network adapters and their drivers will simply pass VLAN tags to the upper layer to handle them. In these cases, Wireshark will see VLAN tags and present them.

In more sophisticated adapters and drivers, the VLAN tag will be handled in the adapter itself. This includes some of the most common adapters with Intel and Broadcom Gigabit chipsets. In these cases, you will have to disable the VLAN feature.

![](bbox=[0.074, 0.325, 0.565, 0.694])

When configuring the NIC driver in order to ensure that it will not handle VLAN tags, packets will simply be forwarded to the WinPcap driver and presented by Wireshark.