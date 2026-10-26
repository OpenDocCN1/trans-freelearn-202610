# There's more...

Here is a short example to understand the principle of ARP operation. In the following diagram, we see an interesting case: two devices attached to the same LAN with different subnets with a default gateway configured to their own IP address. The question is: will a Ping (or any communication) work between them?

![](bbox=[0.07, 0.22, 0.69, 0.38])

Well, intuitively, you will say no because these are two devices on different subnets and therefore require a router between them. But let’s look at what actually happens over the wire:

+   - **192.168.1.10** wants to send a packet to **192.168.2.20**. Since the default gateway is configured to its own address, it thinks the destination is on the same LAN and sends an ARP request.
+   - Since we are on the same LAN as the destination, the destination receives the ARP request (because it is a broadcast) and answers to **192.168.1.10** with its MAC address.
+   - Now, the source has the MAC address of the destination. It sends the packet to it, and although not on the same IP subnet, the destination receives the packet.

Another important issue is Proxy ARP. While a proxy in communications is a device that performs an operation on behalf of someone else, Proxy ARP is the technique in which one host, usually a router, answers the ARP requests intended for another machine.

The proxy ARP concept is implemented in various cases, for example:

+   - While placing a device in front of a router, for example, WAN acceleration/optimization device. When you configure this device in bridge (or