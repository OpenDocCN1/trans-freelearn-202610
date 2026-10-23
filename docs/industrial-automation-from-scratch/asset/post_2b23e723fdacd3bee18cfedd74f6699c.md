
## Questions

The following are some questions to test your understanding of this chapter. Ensure you have read and understood the topics in this chapter before attempting the questions:

- 1. In a/an __________ process control, the output does not change the control action.
- 2. In a/an __________ process control, the output has effect on the control action.
- 3. In a/an __________ control, disturbance is measured and corrective action is taken to correct any errors before they reach the system.
- 4. A __________ is a device that converts a signal produced by a sensor into a standard signal (4 to 20 mA, 0 to 10 V, or 3 to 15 psi) applicable in process control.
- 5. __________ can be referred to as the target value of the process variable.
- 6. __________ is the difference between the process variable or measured value and the set point.
- 7. The devices that the controller operates to keep the process variable at a setpoint are called __________.
- 8. ISA is an acronym that stands for ______________________________.
- 9. __________ is a flow meter that measures the flow rate by measuring the speed of an internal rotating turbine or gears.
- 10. __________ is an inbuilt function of an S7-1200 controller for converting any given data from the minimum to the maximum into 0 to 1 in float values.
- 11. __________ is an inbuilt function of an S7-1200 controller for converting the normalized value from 0 to 1 into the required range.
- 12. PID is an acronym for ________________.
- 13. P&ID stands for ________________.

## Industrial Network and Communication Protocols Fundamentals

Industrial networking is important in industrial automation and process control. Industrial automation provides interconnection between devices (sensors, actuators, controllers, and PCs) and also enables communication between them. An industrial network differs from traditional communication, which allows communication between computers and their peripherals such as printers, scanners, and more. An industrial network is a powerful network that connects industrial automation and control devices (PLCs, HMIs, PCs, VFDs, transmitters, and more) and provides real-time monitoring, control, and data integrity in a harsh environment.

By reading this chapter, you should have a better understanding of industrial networks and the various topologies and communication media. You will learn about the basics of wireless networks and understand what 5G is. You will also learn about industrial network protocols, which are important for communication to occur. Finally, we will provide a practical demonstration of a simple industrial network using the PROFINET protocol.

In this chapter, we will cover the following topics:

- Understanding industrial networks
- Understanding network topology
- Network media – wired and wireless (Bluetooth, Wi-Fi, and cellular communication – 1G, 2G, 3G, 4G, and 5G)
- Network connectors and other network components
- Understanding network protocols
- Common industrial network protocol – Foundation Fieldbus
- Common industrial network protocol – PROFIBUS
- Common industrial network protocol – Modbus
- Common industrial network protocol – HART
- Common industrial network protocol – PROFINET

### Understanding industrial networks

When two or more devices are connected for data exchange, it is referred to as a network. Each device in the network can be referred to as a node. Industrial networks consist of controllers (PLC, PAC, or DCS), field devices, PCs, and other industrial devices that are connected to share and exchange data. A node in an industrial network can be a controller (PLC, PAC, or DCS), field device (sensor/transmitter or actuator), HMI, server, or workstation. The nodes in an industrial network utilize a special protocol for communication and to automate processes in the industry.

### Understanding network topology

Network topology is the bedrock of a network. It shows how the nodes are arranged and interconnected. It refers to the physical and logical arrangement of a network. Hence, network topology can be grouped into two areas:

- **Logical topology**: This refers to the pattern in which data is being transferred between the nodes in the network. It defines the path that data takes when it is being transmitted. It describes the way data travels in the network.
- **Physical topology**: This refers to the physical layout or arrangement of the nodes in the network. It shows how various nodes in the network are connected.

The following are some common physical topologies:

Point-to-point topology: In a point-to-point topology, a connection is made between two devices with a single wire. There is a direct link between two devices in a point-to-point network, as shown in the following diagram. An example of a point-to-point topology in a home or office is a PC connected to a printer:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_568_0.png)

Figure 13.1 – Point-to-point network between device 1 and device 2

In industrial automation, an example of a point-to-point topology is a PLC connected to a PC when downloading a program to the PLC:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_568_1.png)

Figure 13.2 – PC connected to a PLC when downloading a program

The following diagram shows a PLC connected to an HMI for monitoring and control purposes:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_569_0.png)

Figure 13.3 – PLC connected to HMI for monitoring and control purposes

Bus topology: In a bus topology, each node is connected to a single cable segment. This is called the trunk, backbone, or bus. Termination resistors are used at both ends to improve signal quality. When data is to be transmitted by a node to another node, the data is sent out to all the nodes in the network. However, this will be ignored by all other nodes and accepted by the destination node – that is, a node will only respond to the data that is directed to it. A common example of the bus topology in industrial automation is a network of PLCs, HMIs, and other devices using the PROFIBUS protocol:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_570_0.png)

Figure 13.4 – Bus topology

Star topology: In a star topology, each node is connected to a central device. This can be a switch, router, or hub. When a node wants to send data to another node, it sends the data to the central device, which will then pass the data to the destination node. Devices communicate with each other indirectly through the central device:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_570_1.png)

Figure 13.5 – Star topology

**Ring topology:** As the name suggests, when the nodes in the network are connected, they form a ring. The last node is connected to the first node to form a closed loop, as shown in the following diagram. In a ring topology, data is transmitted from one node to another until it reaches the destination node.

![](img/b10cb271ed113c7ba5bfded0a75d13e0_571_0.png)

Figure 13.6 – Ring topology

In this section, you learned about network topology. You should now understand the point-to-point, star, bus, and ring topologies. In the next section, we will learn about network media.

### Network media – wired and wireless (Bluetooth, Wi-Fi, and cellular communication – 1G, 2G, 3G, 4G, and 5G)

Network media can be defined as the channel of communication used to interconnect nodes on a network. Examples of network media include copper twisted pair cables, copper coaxial cables, and fiber optic cables, which are used in wired networks, and radio waves (Bluetooth, Wi-Fi, cellular communication, and so on), which are used in wireless networks. Hence, a network can be wired or wireless, depending on the communication channel being used:

- **Wired network**: A wired network uses cables such as copper twisted pair cables, copper coaxial cables, and fiber optic cables as a communication channel.

The following figure shows a coaxial cable:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_572_0.png)
