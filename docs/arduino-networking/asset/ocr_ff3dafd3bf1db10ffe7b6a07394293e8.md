# Hardware configuration

It's now time to set up the hardware for this project. At this point, if you followed one of the previous chapters, you should already have your Arduino Ethernet shield plugged to the Arduino board, one Ethernet cable connecting the Ethernet shield, and your Internet router. If that's not the case already, please do so.

The only thing you will have to connect in this project is the DHT11 sensor and the resistor. You can have an overview of the different connections you have to make by looking at the following schematic diagram:

![](bbox=[0.061, 0.244, 0.908, 0.526])

First, plug the DHT11 sensor to the breadboard. Then, connect pin number 1 and 2 of the sensor using the 4.7k ohm resistor.

Now, we are going to connect the power supply. Connect pin number 1 of the sensor to the Arduino 5V, and pin number 4 to the Arduino GND. Finally, connect pin number 2 of the DHT sensor to the Arduino pin number 7.

At the end, the connections should look like the following screenshot: