# # Summary

In this first chapter of the book, we built our first project with the Arduino Ethernet shield, only to check whether the shield was working correctly and whether it could connect to your local network and to the Internet. We only made sure that the Ethernet shield could indeed be connected to the Internet, but this already gave you an overview of everything that you can do with the Ethernet shield. You saw how easy it is to send requests with the Ethernet shield, which is something we will use later in the book to send data from the Ethernet shield to a remote server.

These were the major takeaways from this first chapter:

+   - First, we made sure that all the hardware was correctly set and that the shield was connected to your router via an Ethernet cable.
+   - Then, we built the first sketch of this book simply to test that the Ethernet shield could indeed connect to the local network and the Web. To test this, we built a sketch that connects to the Web and grabs a test page from a remote server.
+   - Finally, we uploaded this code to the board and checked that everything was working correctly by monitoring the status of the connection in the Arduino Serial Monitor.
+   We also defined some strategies on what to do if the sketch didn’t work and the shield could not connect to the Web.

In the next chapter of the book, we are going to build our first application using the Ethernet shield. We are also going to use an Ethernet client, but this time to connect to a server running on our own computer and to send measurements that come from a temperature and humidity sensor.