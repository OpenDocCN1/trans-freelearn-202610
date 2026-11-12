# Home automation using a Raspberry Pi

Our home automation project will loosely continue from the “light an LED” example in Chapter 5, Creating, Developing, and Deploying on the Raspberry Pi. It will adapt and evolve that project to create one where the end user can wirelessly light a lamp using a smartphone (or some other device with a web browser).

As illustrated in the following diagram, the idea is to connect a one-channel relay module to the Raspberry Pi, using the GPIO pins. Then, we connect a lamp and an external power source to the relay module, creating a circuit. When everything has been connected, it will be possible to control the relay module (on/off) using GPIO connections from the Raspberry Pi.

![](bbox=[0.0, 0.27, 1.0, 0.53])

A home automation project that enables remote lighting