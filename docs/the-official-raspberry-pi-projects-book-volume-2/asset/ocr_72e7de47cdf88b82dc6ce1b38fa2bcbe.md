# GET STARTED WITH SENSE HAT

The exciting new sensor add-on for the Raspberry Pi is very easy to use once you know how. We’ll show you how, then.

# You'll Need

+   - Sense HAT magpi.cc/1TGGUt5
+   - Sense HAT library
+   - Raspbian - magpi.cc/1MYYTMo

This issue we’ve had a bit of a Sense HAT blow out, with our interview with the creators early on in the mag, and a review of it later on. It’s an amazing piece of kit and hopefully by now you’ve been able to snag yourself one.

While it’s definitely easy to use, there’s a few things you need to learn first to get it going as it’s reliant on programming it via Python scripts at the moment. The commands are very simple though, so we can show you in only a couple of pages how to start getting the most out of your new toy!

Learn how to use the features of the Sense HAT, and figure out how to make this smiley face

All you need is a Raspberry Pi to use the Sense HAT - no other gadgets!

![](bbox=[0.06, 0.61, 0.57, 0.94])

# >STEP-01
Installing the Sense HAT

Getting the Sense HAT ready to use is quite simple. Turn off your Raspberry Pi and make sure your Raspbian SD card is inserted if it isn’t already. Place the Sense HAT on the GPIO pins, carefully aligning them before pressing down firmly so that it properly attaches to the Raspberry Pi. Once that’s done, turn the Raspberry Pi back on. If it’s attached properly, the LEDs on the SenseHAT will light up in a rainbow pattern during boot time. When it gets to the desktop, the pattern might turn off but that’s normal.

# >STEP-02
Sense HAT library

The Sense HAT won’t work straight away on every Pi. You may need to download a custom script that will install the necessary libraries for Python to access it if you’re using a much older version of Raspbian, and update some of the core kernel code so that it runs properly. You can download and install the update by opening the terminal and running these commands:

```
$ sudo apt-get install sense-hat
$ sudo pip-3.2 install pillow
```

After both commands have run their course (they might take a few minutes each to complete) you’ll need to reboot the Raspberry Pi. You might as well do this from the command line with:

```
$ sudo reboot
```

# >STEP-03
Get started with Python

We can start controlling the Sense HAT by writing code in Python. As the Sense HAT is using the GPIO pins, we need to have administrative privileges to execute the scripts. This means opening IDLE using