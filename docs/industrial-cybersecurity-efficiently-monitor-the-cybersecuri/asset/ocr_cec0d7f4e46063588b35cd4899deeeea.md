# Hardware hacking – attacking devices via JTAG

**Joint Test Action Group** (**JTAG**) is an industry standard that’s used to verify electronic circuit designs and test **Printed Circuit Boards** (**PCBs**) once they’ve been manufactured. **JTAG** describes the standards for on-chip instrumentation and testing for **Electronic Design Automation** (**EDA**), which can be used as an additional tool besides digital simulation. It details the use of a dedicated debug port that’s present on the PCB and implemented as a serial communications interface. This allows for low-overhead access to the PCB circuitry debug functionality and firmware, without direct external access to the circuit’s address and data buses being required. The JTAG interface allows connectivity via an on-chip **Test Access Port** (**TAP**). This TAP exposes a stateful protocol, allowing the tester to access a set of registers that represent the chip’s logic levels and the device’s capabilities. Additionally, the JTAG interface allows us to read and sometimes write chip software and firmware.

From a hacking perspective, JTAG is interesting because it allows us to debug a device at a physical level or extract a device’s firmware, among other cool tricks.

Finding the JTAG interface is often the most difficult part of the process. Many companies started obfuscating the JTAG interface on their PCBs with the aim of making things difficult for the reverse engineer. As an example, the following image shows the unmarked JTAG connector location inside a MicroLogix 1100 PLC:

![](bbox=[0.23, 0.53, 0.74, 0.88])

Figure 17.43 – JTAG interface location on a 1756-L61 controller