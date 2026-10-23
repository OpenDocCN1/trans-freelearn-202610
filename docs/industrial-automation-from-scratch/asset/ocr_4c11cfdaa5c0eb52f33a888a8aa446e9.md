![](bbox=[0.126, 0.094, 0.364, 0.356])

Figure 13.24 – PLC and HMI connections using the point-to-point topology

If more than two nodes are needed in the network, we can implement a star topology, where each node (PLC 1, PLC2, PLC 3, HMI 1, HMI 2, and any other devices) will connect to a central component (switch). PROFINET also supports the bus topology.

Devices in a PROFINET network have three addresses (IP address, MAC address, and device name). We implemented a very simple network using the PROFINET protocol in *Chapter 10, Understanding Human Machine Interfaces (HMIs)*, where we connected a Siemens S7 1200 PLC (CPU 1211C) to a Siemens HMI (KTP 400) for monitoring and control. Here, the PLC and HMI, which are the nodes in the network, had unique device names – that is, PLC_1 and HMI_1 (*Chapter 10, Understanding Human Machine Interfaces (HMIs)*, in *Figure 10.24*). Each device in the network was assigned a different IP address (192.168.0.4 for the PLC and 192.168.0.5 for the HMI).

The following figure shows a PLC connected to an HMI for monitoring and control using PROFINET. Refer to *Chapter 10, Understanding Human Machine Interfaces (HMIs)*, to learn more about this configuration: