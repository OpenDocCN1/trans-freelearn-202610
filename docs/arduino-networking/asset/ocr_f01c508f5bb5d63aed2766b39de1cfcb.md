# Chapter 3. Data Logging Station

In this chapter, we are going to create something really similar to what we did in Chapter 2, Sending Data to a Web Server, that is measuring data from a sensor, storing the data, and sending it back to a web server so that it can be plotted.

However, things are going to be similar only on the appearance front. Instead of having an Ethernet client that runs on the Arduino board and sends data to a server, we are going to make the Arduino board more independent. In the first part of the chapter, we are going to log the data locally using the integrated MicroSD card reader of the Ethernet shield.

Then, we are going to create a server right on the Arduino board. The board will measure data as usual, but the server will serve the data to the incoming clients. Finally, we will use a modified version of the code we used in the Chapter 2, Sending Data to a Web Server, to plot the measured data on your computer.

The following will be the major takeaways of this chapter:

+   First, we are going to build the hardware for this project, including the temperature and humidity sensor, and the SD card to log the measured data.

+   Then, we will create the first part of the project, and log data automatically on the SD card, which is inserted inside the Ethernet shield. At this point, we are going to use the Internet connection of the Ethernet shield to automatically obtain the current time from a Network Time Protocol (NTP) time server.

+   Finally, we are going to build a web server and run it on the Arduino board. You will then be able to access the data measured by the board just by entering in the address of the board in a web browser. We are also going to modify the code from the last chapter to obtain the measurements from the Arduino board and plot them live in your browser.