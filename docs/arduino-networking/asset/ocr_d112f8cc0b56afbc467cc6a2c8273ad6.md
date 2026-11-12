# # Chapter 4. Controlling Objects from Anywhere

In this chapter, we are going to do something different to what we did in the other chapters. What we did so far was measure some data on the Arduino board and transmit this data back to the network using the Ethernet shield. In this chapter, we are going to control a device instead of measuring data.

To do so, we will use a relay connected to the Arduino board with the Ethernet shield and control this relay remotely. We are also going to connect a set of power cables to this relay so you can directly plug a lamp into it so that it can be controlled remotely.

In the first part of the chapter, we are going to control this relay remotely within your local network. In the second part of the chapter, we will use a dedicated web service to control this relay from anywhere in the world.

The following will be the major takeaways from this chapter:

+   • First, we are going to see how to choose the different components for this project, including the relay and cables to connect a device to the relay such as a lamp.
+   • Then, we will assemble the different hardware components, connect the relay to the Arduino board, and the power cables to the relay.
+   • After that, we will write an Arduino sketch to test whether the relay is working correctly and has been correctly connected to the circuit.
+   • When we are sure that the relay is working, we will write an application using the Ethernet shield along with the relay. We are going to write a sketch to control the relay via Ethernet and then build an interface to control the relay from your web server.
+   • Finally, in the last section of the chapter, we are going to use a dedicated web service to control the relay from anywhere in the world. We are also going to adapt the interface we just created to control the relay from anywhere.