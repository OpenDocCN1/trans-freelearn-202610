# THE MECHANICAL PROCESS

![](bbox=[0.01, 0.04, 0.56, 0.4])

![](bbox=[0.57, 0.17, 0.93, 0.33])

# >STEP-01
On standby

The power switch is controlled by an ATtiny chip that stays in sleep mode until the power is switched on, which then wakes it up so it starts to provide power to the screen.

![](bbox=[0.57, 0.43, 0.93, 0.62])

# >STEP-02
While it's on

The Raspberry Pi has a piece of code that lets the ATtiny know that it is on, and it boots straight to RetroPie. The power switch begins a shutdown procedure.

![](bbox=[0.57, 0.7, 0.93, 0.86])

# >STEP-03
Turning off

When the ATtiny is told the Pi is shutting down, it turns off the screen and grounds the power, turning off the regulator before going back into sleep mode.

![](bbox=[0.01, 0.4, 0.56, 0.64])

The buttons on the rear are very basic, but Chris is unsure how to improve them

> Enough battery life for seven hours of play while being smaller than the original Game Boy!

switches cannibalised from a NES and several Game Boys.
“It was complex to get everything to fit and work together,” Chris admits.
“I designed the housing in SolidWorks, wrote programs for the Pi in Python, programmed the AVR in C, designed custom circuit boards, lots of breadboard time, some talking to Adafruit to see what their products can and can’t do, etc. But it was relatively easy to build.”

The result works well enough for Chris, with power on and off switches, games loading from a USB port on the side, and the ability to charge the battery from the bottom of the case with a micro-USB cable.
Right now, Chris is making a second one for his girlfriend and will eventually write up a full rundown on how to make your own, including slightly improved plans for the circuit boards. Soon you’ll be able to make long trips much more interesting.