# # Serial and SSH connections to the Raspberry Pi

The first method is to use a serial connection to the Raspberry Pi. The easiest way to achieve this is to use a USB-to-serial cable. Connect the USB to the your host machine. Then, connect the four wire cables to the correct GPIO pins on the Raspberry Pi, as showed in the following figure:

![](bbox=[0.0, 0.145, 0.86, 0.735])

A USB-to-serial cable connected to the Raspberry Pi GPIO header

The following table will give a more detailed explanation of how the GPIO wires should be connected.
Also check the GPIO header diagram in Chapter 8 , Diving into the Raspberry Pis Peripherals and Yocto Recipes. to review the purpose of all the pins in the GPIO header.

| Red | 5V |
|---|---|
| Black | GND |