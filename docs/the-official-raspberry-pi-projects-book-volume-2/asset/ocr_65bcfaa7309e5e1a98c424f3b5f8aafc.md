# MAGIC PRESENTATIONS WITH SKYWRITER

Amaze your audience with baffling magical powers or mastery of the Force using a Skywriter HAT

# RICHARD HAYLER

Richard is a mentor at CoderDojo Ham. His school CodeClub was one of the winning teams in the Primary School Astro Pi competition. magpi.cc/1ILmeoi coderdojoham.org

![](bbox=[0.61, 0.07, 0.68, 0.15])

# You'll Need

+   - Skywriter HAT or board shop. pimoroni.com
+   - Skywriter API library magpi.cc/2dXntBn
+   - Python AutoPy library magpi.cc/2dXmbpZ
+   - LibreOffice Impress libreoffice.org

Below You can use longer wires or a ribbon cable to locate the Skywriter board away from the Pi if needed

Let’s face it: simply using a Raspberry Pi to run the slides for your talk will make the audience think you’re pretty cool. But if that’s not impressive enough, why not dazzle them further by using your telekinetic powers to flip through the presentation and annotate the slides by drawing in the air? You don’t even need to be a graduate of Hogwarts or the Jedi Academy: just get yourself a Skywriter device and some simple Python, and you’re ready to rock. The Skywriter device uses a grid of transmitting electrodes to generate an electric field that propagates around the surface in three dimensions. When you move your hand above the Skywriter, it disturbs this field and these variations are detected by the receiver electrode grid. These measurements can be used to calculate the position and movement direction of your hand.

# >STEP-01 Connect the Skywriter device

If you have a Skywriter HAT, this just connects onto the GPIO pins like other HATs. If you have the larger Skywriter board, you’ll need to connect six GPIO pins to the matching pins at the top, as shown below.

![](bbox=[0.07, 0.72, 0.58, 0.94])

# >STEP-02 Install the software

Make sure you have the latest version of Raspbian, with all updates installed. As usual, those helpful Pimoroni Pirates supply a single script to handle the installation, including the full Python API. Like most HATs, the Skywriter needs the I²C bus on the Pi to be enabled, so if you haven’t already got this activated on your Pi, you’ll need to reboot before the Skywriter will work.

```
curl -sSL get.pimoroni.com/sky writer | bash
```

You’ll also need the AutoPy Python library and its dependencies, so install these with:

```
sudo apt-get install libx11-dev libxtst-dev
```

...and then:

```
sudo pip install autopy
```

# >STEP-03 Test your Skywriter

The Python API has example scripts to help you become familiar with the way Skywriter works:

```
cd Pimoroni/sky writer
sudo python test.py
```

Now wave your hand around in the air just above your Skywriter. You should see three columns of scrolling numbers corresponding to your hand’s position in a three-axis (x/y/z) box over the device. The Python library is preconfigured to recognise certain gestures: a flick (swiping over the Skywriter), a tap or touch (bring your hand down to just above the surface), and, trickiest of all, the Airwheel (wiggle a finger in a circular pattern above the Skywriter). It takes a while to get the hang