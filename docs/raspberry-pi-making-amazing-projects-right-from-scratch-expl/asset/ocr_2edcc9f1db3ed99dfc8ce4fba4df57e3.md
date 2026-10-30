The fact that it just switches a relay on and off means that we can easily implement this in our home security system because it simply acts as a switch. When you press the button on the transmitter, the relay switches the contacts on; press it again, and the relay switches off. The screw terminals on the board provide us with access to the relay terminals.

# The receiver wiring diagram

As we are just dealing with a switch input, we can use the same circuit as we did with the zone circuit earlier but connected to our arm/disarm GPIO input, which we'll determine in Chapter 9, Putting It All Together.

![](bbox=[0.25, 0.33, 0.75, 0.6])

Circuit to interface the receiver module with a GPIO input

When the receiver module switches the relay on, this will complete the 12V circuit through the opto-coupler's LED by turning it on. This will make the transistor pull the GPIO pin down to ground, giving it a low input.

You can use this type of circuit for any paired receiver for the wireless security devices that you want to use in your system.