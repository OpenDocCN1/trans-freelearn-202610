The client with the assigned Media Access Control [MAC] address will now always receive the set reserved IP. The client will not automatically receive this IP address. The client will receive the reserved IP address in the following scenarios:

+   - Renewal of current lease for the client
+   - A reboot of the client
+   - Running the ipconfig command on the client, like so: ipconfig /release and then ipconfig /renew

Reserved IP addresses for a client can be changed; the device is just assigned a new reservation, and the current one is removed so that IP can be used again for other clients.

# DHCP options

Through a capability known as Options, DHCP can provide additional network resource information required to be set on a machine’s network interface to communicate on the network. These settings are managed via the DHCP console.

The following screenshot represents this:

![](bbox=[0.13, 0.46, 0.87, 0.67])

Figure 5.14 – DHCP options

These network settings typically apply to the scope level, which is the subnet for clients; they can also be configured to apply at the server or class level to meet your needs. The scope- and server-level options are both overridden by the class option.

The following are common DHCP options that could be set at the scope level, to be set as network settings on the clients:

+   - DHCP option 1: subnet mask
+   - DHCP option 3: router (default gateway)