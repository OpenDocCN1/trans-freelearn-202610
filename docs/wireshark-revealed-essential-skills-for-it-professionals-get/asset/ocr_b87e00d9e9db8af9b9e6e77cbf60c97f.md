# Selecting the correct network interface

As discussed in Chapter 1, Getting Acquainted with Wireshark, if you have multiple network interfaces on your machine, you need to determine and select the correct interface to capture packets. In Wireshark's Capture menu, click on Interface or click on the first icon on the icon bar.

The Wireshark Capture Interfaces window provides a list and description of the network interfaces on your machine, the IP addresses assigned, and the total packets and packets per second counters for each interface. If an interface has an IPv6 address assigned and this is being displayed, you can click on the address to toggle and display the IPv4 address.

The following screenshot illustrates a typical Capture Interfaces window listing a LAN and wireless interface along with their IP addresses and packet counters:

![](bbox=[0.07, 0.41, 0.93, 0.57])

The Capture Interfaces window provides the following two options:

+   - Clicking on the Details button for any of the listed interfaces opens an Interface Details window that provides a wide range of information that can be useful to verify the interface's operation. The status of the Link and Link Speed information is displayed in the Characteristics tab, and the MAC address of the selected NIC is displayed in the 802.3 (Ethernet) tab.

+   - The rest of the capture options are configured in the Capture Options window, which is opened by clicking on the Options button in the Capture Interfaces window, or by selecting Options from the Capture menu, or by clicking on the second icon in the icon bar.

The following screenshot illustrates a typical Capture Options window with a number