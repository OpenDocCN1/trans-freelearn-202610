This sensor is less expensive and easy to use; it takes a bit of processing power to coordinate the efforts of timing the send and receive signals, but the Raspberry Pi B 2 has the processing power needed. Here are the steps to set up this sonar sensor to sense the distance:

+   1. The first step is to understand the GPIO pins of the Raspberry Pi B 2. Here is a diagram of the layout of the pins:

![](bbox=[0.386, 0.267, 0.614, 0.571])

In this case, you'll need to connect to the 5 volt connection of the Raspberry Pi B2, which is pin 2. You'll also need to connect to the GND, which is pin 6. You'll use pin 16 as an output trigger pin and pin 18 (GPIO24) as an input to time the echo from the sonar sensor.