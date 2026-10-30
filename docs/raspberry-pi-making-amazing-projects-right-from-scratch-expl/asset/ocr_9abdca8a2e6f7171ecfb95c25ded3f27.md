![](bbox=[0.378, 0.147, 0.621, 0.295])

Official Raspberry Pi USB Wi-Fi Dongle

On earlier Raspberry Pi models, the amount of current that the ports delivered was pretty low and caused all sorts of problems if too much current was drawn by the connected devices. This was significantly improved from the model B+ onwards, and it's now possible to connect GSM/LTE dongles without any problems.

There are still limitations, however, if you want to connect things such as hard disk drives; these can still draw more current than what can be supplied by the Raspberry Pi USB ports, so it's recommended that a powered USB hub or USB power injector be used when connecting these types of devices to your Pi.

# Power connections

The GPIO connector also provides access to the on-board power supplies. The +5V connection (pins 2 and 4) is essentially the +5V input from the external power supply connected to the micro-USB power port. This can be used to power small external circuits if necessary, although it is recommended that an additional external +5V supply be used if significant current is required.

The +3.3V supply (pins 1 and 17) is the output from the on-board 3.3V regulator and provides a small amount of current up to 50mA. If you need to draw more than 50mA for your external circuits, then you should use an external power supply. I'll show you how to build one later in this book.

> The I/O pins on the Raspberry Pi operate at 3.3V levels. Connecting voltages higher than this to the pins could irreversibly damage your Pi. If you follow the instructions in this book, then everything should be fine, but randomly connecting things to your Pi that use lots of power will break it!