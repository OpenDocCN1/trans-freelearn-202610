+   - Understanding HMI programming software
+   - Exploring HMI manufacturers and their programming software
+   - Interfacing PLCs and HMIs
+   - Downloading programs to PLCs and HMIs

# Technical requirements

You will need an understanding of PLCs, sensors, and actuators from the previous three chapters.

Ensure you have downloaded and installed TIA Portal V13 on your PC or laptop. Please refer to Chapter 8, Understanding PLC Software and Programming with TIA Portal.

Other devices that may be required include Siemens HMI (KTP400) and Siemens PLC (S7 1200, CPU 1211C, AC/DC/RLY).

# Introducing HMIs

HMIs can be seen as graphical interfaces that allow humans and machines to interact. We have learned that PLCs can control and automate industrial machines. HMIs, on the other hand, allow humans to interact or communicate with machines. No matter how we automate a process, we still need a human (an operator) to start or initiate the process, perform other functions, and get feedback on the machine’s operation.

HMIs allow humans to give commands to PLCs to control a process and also receive feedback from PLCs about the process. With HMIs, the human operator can interact with, monitor, and control machines via the PLC. The HMI is usually connected to the PLC via a communication cable, depending on the protocol being used (Ethernet/IP, Profinet, Profibus, or Modbus). This chapter does not discuss protocols. We will look into protocols in Chapter 13, Industrial Network and Communication Protocols Fundamentals. However, we will use Profinet as the protocol to establish