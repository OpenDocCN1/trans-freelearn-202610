![](bbox=[0.0, 0.09, 1.0, 0.36])

![](bbox=[0.6, 0.07, 0.68, 0.15])

# JAMES SINGLETON

James is a software developer and engineer. He founded Computing 4 Kids Education (C4KE) and Yo Flow online gym inductions. computing4kids.com unop.uk

# TIME-LAPSE PHOTOGRAPHY

Photo by NASA JSC

Make a device to capture photographs at regular intervals. Then turn these images into a video

# You'll Need

+   - Raspberry Pi Camera Module
+   - magpi.cc/1UaFuuW

Time-lapse photography reveals exciting things about the world which you wouldn’t otherwise be able see. Things that happen too slowly for us to perceive: bread rising and plants growing; the clouds, sun, moon, and stars crossing the sky; shadows moving across the land. In this tutorial, we’ll be making a Raspberry-based device that lets you watch things that are too slow to observe with the naked eye. To do this, we will capture lots of still photographs and combine these frames into a video with FFmpeg/libav, which can then be accessed via a web browser.

The top of the camera is at the other end of the ribbon cable connection

The metal contacts on the ribbon cable should face away from the Ethernet socket

![](bbox=[0.16, 0.76, 0.57, 0.94])

# >STEP-01

# Connect the Camera Module

First, connect the camera module to the Raspberry Pi with the included ribbon cable. Locate the correct socket; it’s on the top of the Raspberry Pi circuit board and is the one furthest away from the micro-USB power connector. The socket is handily labelled ‘CAMERA’ on the newer Raspberry Pi models. Lift up the outside of the socket to release the clamp, then insert the ribbon cable with the metal contacts facing towards the micro-USB power connector. Finally, hold the ribbon cable in position and push the outside of the socket back down to clamp the cable in place.

# >STEP-02

# Enable and test the camera

Power the Raspberry Pi up. You now have a choice: boot to the command line, open a terminal window, or establish a secure shell (SSH) connection. Enable the camera by running this command from a terminal to launch the Raspberry Pi configuration tool:

```
sudo raspi-config
```

Then select the 'Enable Camera' option. You can test the camera by running the following command: