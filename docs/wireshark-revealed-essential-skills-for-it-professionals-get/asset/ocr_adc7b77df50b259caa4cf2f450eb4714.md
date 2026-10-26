# How it works...

First, there are two different definitions; it is important to distinguish between:

+   - **Bandwidth**: This is the total bits per second that can be transferred over a communications line
+   - **Throughput**: This is the effective application bytes per second that is transferred between the two ends of a connection

To check the bandwidth of a communication line, you can ask the service provider for the line details, or you can simply transfer some traffic over it, use Wireshark or SNMP tool, and see what you get.

Most of the cases in which a duplex mismatch problem occurs is when you connect using Ethernet on one side with 100 Mbps full duplex, and the other side configured to auto-negotiate.

![](bbox=[0.07, 0.42, 0.81, 0.59])

As you see in the diagram, when you connect a device (a router in this example) to a switch, when both sides are manually configured, for example, to 100 Mbps Full Duplex (FDX), the intended configuration will take place (numbered 1 in the preceding diagram).

When you configure both sides to auto-negotiation (numbered 4 in the preceding diagram), it will also be fine, and will be automatically set to 1 Gbps (in the case of gigabit adapters).

In the case when one side is configured to 100 FDX and the other side to auto negotiate, the auto negotiate will be automatically set to 100 Mbps Half-Duplex (HDX). In this case, when one side is set to HD and the other to FD, many packets will be lost, and you