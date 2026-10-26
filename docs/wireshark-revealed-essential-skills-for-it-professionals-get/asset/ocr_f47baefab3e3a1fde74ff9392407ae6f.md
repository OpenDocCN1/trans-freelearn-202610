# There's more...

For Spanning Tree debugging, the best thing is to get the data from a direct connection to the LAN switches. A well-configured SNMP trap to a management system can also assist in this task.

Some examples of STP packets are as follows:

+   - In the following screenshot you see an STP frame. You can see that the source MAC address is a Nortel address, and in the BPDU itself you see that the root and the bridge identifiers are equal; this is because the bridge that sends the packet is the root. The port ID is 8003, which in Nortel switches indicates port number 3.

![](bbox=[0.121, 0.324, 0.928, 0.605])

+   - In the following screenshot, you can see a Rapid STP BPDU. You can see here the protocol identifier that equals 2 and the port state that is designated.