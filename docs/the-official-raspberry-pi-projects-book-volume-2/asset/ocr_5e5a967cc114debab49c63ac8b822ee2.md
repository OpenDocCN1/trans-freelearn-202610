# Tutorial

# WALKTHROUGH

# EVERYDAY ENGINEERING PART 12

# SIMON MONK

Simon Monk is the author of the Raspberry Pi Cookbook and Programming the Raspberry Pi: Getting Started with Python, among others.
simonmonk.org
monkmakes.com

![](bbox=[0.62, 0.07, 0.75, 0.16])

# BINARY

# CLOCK

# You'll Need

+   - Raspberry Pi with 40-pin GPIO header
+   - Sense HAT or Pimoroni Unicorn HAT
+   - USB WiFi adaptor or Ethernet connection

Solve real-world electronic and engineering problems with your Raspberry Pi and the help of renowned technology hacker and author, Simon Monk

This is a very simple project to make. In fact, all you really need to do is plug the Sense or Unicorn HAT onto the Raspberry Pi’s GPIO header and install the software.

This project displays the current time in binary. The rows of the display are (from top to bottom) the last two digits of the year (e.g. 15), the month number, the day of the month, the hour (24-hour format), the minute, the second, and finally hundredths of a second.

# Reading the binary clock

To read a binary clock, you either need to be good at recognising binary numbers or use a bit of arithmetic to calculate the time, by adding up the binary digit values for the LEDs that are lit on a particular row.

For example, the first row displays the year in two-digit format. So, if LEDs at positions 8, 4, 2, and 1 are all lit, then the year is 8+4+2+1 = 15. In 2016, just the digit position 16 will be lit.

The colours of the LEDs have no significance – they are different to make the display more interesting. If you’re using the Sense HAT, you may need to install the software for the HAT using the following commands:

![](bbox=[0.6, 0.2, 0.99, 0.72])

```
sudo apt-get update
sudo apt-get install sense-hat
sudo pip-3.2 install pillow
```

If you’re using a Unicorn HAT, then run the following commands:

```
sudo apt-get install python-pip python-dev
sudo pip install unicornhat
```

There are two versions of the software for this project – one for the Sense HAT and one for the Pimoroni Unicorn HAT – so be sure to use the right version of the program for your hardware. The Sense