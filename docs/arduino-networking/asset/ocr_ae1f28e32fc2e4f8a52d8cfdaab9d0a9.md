# # Hardware configuration

Now let's see how to connect the different components of the project. As with the other chapters of the book, you will need to have the Ethernet shield plugged into the Arduino board. You will also need to connect an Ethernet cable between the Ethernet shield and your router.

Then, you need to connect the relay module. The relay module has three input pins: VCC, GND, and a signal pin usually called SIG. You need to connect the VCC pin to Arduino 5 V, GND to Arduino GND, and finally the signal pin to Arduino pin number 7, which is a digital pin. The following image shows the system at this point:

![](bbox=[0.061, 0.264, 0.783, 0.605])

Then, you need to connect the power cables to the relay module. A relay module basically has three output pins: COM (for the common pin), NC (for the normally closed pin), and NO (for the normally open pin).

What we want is to have the COM pin of the relay connected to one wire of the power plug, NC not connected, and NO connected to one other wire of the power plug. The following image summarizes the connections at this stage: