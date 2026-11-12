# Controlling the lamp using the Raspberry Pi

At this point, we can control the relay locally from our board. Next, it's time to connect the lamp to our relay module.

# Note

WARNING: This project require basic knowledge of electricals. We strongly discourage using high-voltage power if you don't have the knowledge or experience. The recommendation is to use a low-voltage 12-V power supply.

![](bbox=[0.01, 0.24, 0.17, 0.44])

A one-channel relay module

Before we connect the lamp, we need to go through some basics about how the relay module will be used. Look closely at the following relay; it contains C, NC, and NO openings on the opposite side to where we inserted the GPIO jumper cables. These slots will be used to connect the lamp to the relay.

The C on the relay stands for common connection, and we will connect the power from our external power supply to this port. Next, we have NC and NO, which stand for **normally closed** and **normally open**. In our project, we will be using NO. The reason for this is that it will work like a switch. By default, there will be no contact between C and NO, but when we trigger the relay using the signaling from our GPIO pin, we will turn off the relay and that will open the circuit. Using NC instead of NO will give you the opposite behavior.

The lamp and the external power supply (12V strongly preferred for safety reasons) will be connected as shown in the following figure. Note that if no adapter is used for the lamp, you might need to open the outer shell of the cable to get the positive and negative wires. The black negative cable can be connected right from the external power supply to the lamp. The positive must be inserted from the power supply to the C slot on the relay. Finally, we need to connect the red positive cable from the lamp to the NO slot in the relay to get the required behavior as described earlier.