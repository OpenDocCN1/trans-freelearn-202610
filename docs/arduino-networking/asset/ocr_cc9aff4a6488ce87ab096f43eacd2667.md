# Chapter 2. Sending Data to a Web Server

In the previous chapter, we made sure that your Arduino Ethernet shield could actually connect to your local network and to the Web. In this chapter, we are going to build on that, and build our first application using the Ethernet shield.

We are going to see in more detail how the Ethernet client works by measuring data from a digital sensor and sending this data to a web server. In this chapter, this web server will be a server running on your own computer.

These will be the major takeaways from this chapter:

+   - First, we are going to choose the temperature and humidity sensor that we will also use later in the book. We are also going to install the different software components that are required for this chapter, especially the library to plot data on your computer.
+   - Then, we are going to build the Arduino code that will perform measurements and send these measurements to the web server running on your computer.
+   - After that, we’ll start building the server-side code. In the first part, we are going to build the code responsible for simply logging the received data into a local database.
+   - Finally, we will interface the database with a live plotting library so the measurements can be seen as they come from the Ethernet shield and are logged in the database.