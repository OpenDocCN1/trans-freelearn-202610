# Tutorial

# STEP BY STEP

ROB ZWETSLOOT
Tech writer, avid coder, and Raspberry Pi enthusiast with a history of building many things with Raspberry Pi
magpi.cc

![](bbox=[0.02, 0.09, 0.78, 0.44])

Create simple images or more advanced pixel art to represent your favourite thing

![](bbox=[0.71, 0.25, 0.95, 0.42])

The 8×8 display on the Sense HAT may be simple, but it has a wide range of colours and is fully programmable

# PIXEL ART
ON SENSE HAT

Make use of Python and the Sense HAT’s LEDs to read and display 8×8 pixel art

# You'll Need

+   - Raspbian magpi.cc/1MYYTMo
+   - Sense HAT magpi.cc/1TGGUt5
+   - Pixel art

# INSPIRATIONAL CODE

This tutorial was inspired by reader Mike Legg on Twitter, who created code to cycle through all 100 characters on Johan Vinet's sheet. View the Python code for this here: bit.ly/1GOVoBp

We know what you’re thinking: 8×8 pixel art doesn’t seem like a whole lot of pixels. And if you were thinking that, you’d be right. That’s 64 pixels to try to convey something – simple for low-complexity objects, but for proper pixel art it seems like a tall order. If you think back to the early days of gaming, though, there are plenty of examples of sprites that didn’t use many pixels at all and looked… passable. The main limitations in those days were the number of colours you could have on screen at once, and with the Sense HAT we have the power to use a full RGB spectrum, utilising every pixel to make something a bit better than on the old Atari 2600. Grab a sprite, and let’s go!

# >STEP-01
Get the Sense HAT ready

If you’ve upgraded to the newer version of Raspbian, Jessie, all you need to do is switch off the Raspberry Pi, remove the power cable, and then plug the Sense HAT on top of the Pi. Turn it back on and it’s ready to use. If you haven’t made the update, you’ll need to install a few extra libraries first. Open the terminal and run the following commands:

```
sudo apt-get install sense-hat
sudo pip-3.2 install pillow
```

...and then reboot the Raspberry Pi (which you can also do in the terminal with sudo reboot if you wish!)

# >STEP-02
Find some art

There’s a few ways you can do this: using Google or searching on something like DeviantArt with the right keywords (‘8×8 pixel art’ is a good start) and you should be able to find a suitable sprite for showing off on your Sense HAT. You could also experiment with a square picture (from something like Instagram) to see what the code will spit out, or you can draw your