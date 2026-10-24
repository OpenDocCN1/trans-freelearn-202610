# Adding a sonar sensor

The basic circuit you just built is a wonderful start. Now you'll interface a more complex device, a sonar sensor, with Raspberry Pi Zero. Here is a picture of the sonar sensor that you'll add:

![](bbox=[0.29, 0.24, 0.71, 0.44])

The device is an HC-SR04 and they are available at most online electronics retailers. Now let's connect the device to Raspberry Pi Zero. In order to do this, first let's look at the layout of the GPIO pins on Raspberry Pi Zero:

| Pin 1 3.3V |  | Pin 2 5V |
| Pin 3 GPIO2 |  | Pin 4 5V |
| Pin 5 GPIO3 |  | Pin 6 GND |
| Pin 7 GPIO4 |  | Pin 8 GPIO14 |
| Pin 9 GND |  | Pin 10 GPIO15 |
| Pin 11 GPIO17 |  | Pin 12 GPIO18 |
| Pin 13 GPIO27 |  | Pin 14 GND |
| Pin 15 GPIO22 |  | Pin 16 GPIO23 |
| Pin 17 3.3V |  | Pin 18 GPIO24 |
| Pin 19 GPIO10 |  | Pin 20 GND |
| Pin 21 GPIO9 |  | Pin 22 GPIO25 |
| Pin 23 GPIO11 |  | Pin 24 GPIO8 |
| Pin 25 GND |  | Pin 26 GPIO7 |
| Pin 27 ID_SD |  | Pin 28 ID_SC |
| Pin 29 GPIO5 |  | Pin 30 GND |
| Pin 31 GPIO6 |  | Pin 32 GPIO12 |
| Pin 33 GPIO13 |  | Pin 34 GND |
| Pin 35 GPIO19 |  | Pin 36 GPIO16 |
| Pin 37 GPIO26 |  | Pin 38 GPIO20 |
| Pin 39 GND |  | Pin 40 GPIO21 |