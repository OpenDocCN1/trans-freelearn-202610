# Our application - an introduction

Now that we've covered how to generate the SDK for our platform and how to integrate package in our Yocto image, we will, through an example, develop an application with the general purpose input output (GPIO) pins of the Raspberry Pi and then create a recipe to integrate our application with the final image.

The idea is to develop an application that, through the GPIO pins, lights an LED and monitors a push button.

Here is the schematic diagram, realized with Fritzing (http://fritzing.org/home/):

![](bbox=[0.0, 0.27, 1.0, 0.81])

Thus, we can, from the Linux user space, light the LED or monitor the push button through GPIO 4 of the Raspberry Pi (pin 7 of the main connector).

We can test the following C application: