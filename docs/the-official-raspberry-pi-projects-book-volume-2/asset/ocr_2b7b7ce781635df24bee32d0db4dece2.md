# Tutorial
STEP BY STEP

# ROB ZWETSLOOT

Tech writer, avid coder, and Raspberry Pi enthusiast with a history of building many things with Raspberry Pi.
magpi.cc

![](bbox=[0.0, 0.08, 1.0, 0.44])

> The main indication that something is different about this controller, but it's a small price to pay.

> A standard knock-off USB NES controller works perfectly for what we have planned

# NES ZERO

We make last month’s project idea into reality with a NES controller that’s been Zero-charged to be able to run NES games…

# You’ll Need

+   - USB NES controller (check eBay!)
+   - A Raspberry Pi Zero - raspberrypi.org
+   - RetroPie - magpi.cc /z1HVgNba
+   - A rotary tool (like a Dremel)
+   - An old micro USB cable
+   - A screwdriver, soldering iron and a way to strip wires

Ever since first seeing the Raspberry Pi Zero a few months ago at Pi HQ, we’ve spent a lot of time since then thinking about the cool ways that the Pi Zero can be used. One of our first ideas was slipping a Pi Zero into a NES controller, taking a bit of inspiration from Ben Heck’s great inventions to further miniaturise the NES to its most core component.

We wrote about how you could do this last issue but we have now finally had time to make a proof-of-concept, so we’re going to show you how to exactly put a Pi Zero inside an NES controller.

# >STEP-01
Set up RetroPie

This is really more for convenience than anything else. Download the RetroPie image and write it to a microSD card using your preferred method (check the Raspberry Pi guide if you’ve not done this before: magpi.cc/1XTmymk). Once it’s written, open it up in your file explorer and navigate to home, pi, RetroPie and put your NES ROMs into the nes folder.

Pop the micro SD card into the Pi Zero and hook the USB NES controller up to it before turning it on. You can then do the initial setup and configure the controller.

![](bbox=[0.07, 0.77, 0.58, 0.94])

# >STEP-02
Make some space

It seems that many USB NES controllers have roughly the same internal structure (they may be using a mould of the NES controllers, or just a generic one) and it is therefore likely that you’ll need to make some space. We used a rotary tool to trim the screw holes and a ring on the edge. We wanted to use the original USB wire port for our HDMI cable, so we suggest that you lay the Pi Zero (with SD card inserted!) inside the back plate and figure out where needs to be trimmed.