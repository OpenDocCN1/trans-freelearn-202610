
![](img/631e8b200cce187082ee46b0eb1681b3_381_0.png)

The Raspberry Pi NoIR camera module; it looks similar to the standard model

## An illuminating experience

In order to switch on a light or LED array from the Raspberry Pi GPIO or our port expander circuit, we need something that will allow us to drive higher currents and voltages than can be provided by the GPIO ports alone.

A good candidate for this is the TIP120 Darlington transistor, which will allow us to switch on and off loads of up to 80V and 5 A from our GPIO pins. In our full system later on, we're going to use Port B of our MCP23017 port expander to control outputs, but the principle stands for any of the GPIO outputs available to us.

![](img/631e8b200cce187082ee46b0eb1681b3_381_1.png)

TIP120 transistors can be bought cheaply but can drive large loads

The following circuit shows how we can drive big loads from our GPIO port outputs.

![](img/631e8b200cce187082ee46b0eb1681b3_382_0.png)

In our example circuit, we're using a GPIO output pin to control the base of our transistor via a 220 ohm resistor. When the GPIO pin goes high, the transistor is switched on and allows the 12V circuit to flow through the LED array.

In the preceding circuit, there is no current limiting for the LEDs because they are connected in series, and so with nine of them, each dropping about 1.5V across, this is about right for a 12V supply (yes I know I've only included six LEDs here but it's just for illustration). Remember to adjust for your particular needs. This circuit could easily drive other loads, such as bulbs or sounders.

> If you intend to drive high power loads, you will probably need to attach the TIP120 to a heat sink that will dissipate any heat and prevent it from over-heating and burning out. In our circuit that was demonstrated previously, however, you probably won't need one as we're only driving a couple of hundred milliwatts at most.

## The Elaborate light switch re-visited

Expanding once again on our elaborate light switch from previous chapters, we can once again write a Bash script that will switch on our camera light, take a snap with the camera, and e-mail it to us when a PIR detector is triggered.

For the following script, we're assuming that the output controlling the TIP120 transistor is the Raspberry PI GPIO17 pin (D0 or pin 11 of our connector), which replaces the LED in our earlier set-up. The input from the PIR trigger is, again, connected to the GPA0 (port A, data pin 0) of our MCP23017 port expander. All the other inputs are tied low, as before, using 10 K resistors:

```
#!/bin/bash

#set up the High Load GPIO pin
sudo echo 17 > /sys/class/gpio/export
sudo echo out > /sys/class/gpio/gpio17/direction

#set up port expander Port A for inputs
sudo i2cset -y 1 0x20 0x00 0xFF

#clear the output by default to switch light off
sudo echo 0 > /sys/class/gpio/gpio17/value

# loop forever
while true
do
  # read the sensor state
  SWITCH=$(sudo i2cget -y 1 0x20 0x12)

  #PIR is normally closed so pin is held high
  if [ $SWITCH != "0x01" ]
  then
    #PIR was triggered - pin taken low

    #switch on lamp driver
    sudo echo 1 > /sys/class/gpio/gpio17/value
    sleep 0.5

    #take a still image
    sudo raspistill -o -image.jpg -h 768 -w 1024 -q 25

    #email the image
    mpack -s "Security Alert Photo" test.jpg me@mydomain.com

    #switch off the lamp driver
    sudo echo 0 > /sys/class/gpio/gpio17/value
fi
#short delay
sleep 0.5
done

pir-camera-trigger.sh
```

You'll now see that we've started developing the foundations of the software that will control our home security system.

## Is that a badger?

If you don't want to illuminate an area before capturing an image, you can use **infra-red lighting** in conjunction with a compatible camera. The standard Raspberry Pi camera module won't work with infra-red lighting because it contains an infra-red filter, but we can use the NoIR version of the camera module instead.

The Raspberry Pi NoIR camera module is exactly the same as the standard one, except that it doesn't have an infra-red filter built in, which means it will see in the dark with the aid of infra-red lighting. This makes it good for watching badgers at night as well as for use in our home security system.

You will need an infra-red LED array or cluster to invisibly illuminate the area you want to capture with the camera. These are readily available in various form factors and intensities, or you can build your own using individual infra-red LEDs purchased from an electronics store.

![](img/631e8b200cce187082ee46b0eb1681b3_384_0.png)

Connecting and driving the LED cluster modules works exactly the same as our illuminating light above, using the TIP120 driver circuit. The only difference is that we humans can't see when the LEDs are on.

## Using USB cameras

Instead of using the Raspberry Pi Camera Module, it's also possible to use a standard USB webcam to take still images. You should be aware though that the dedicated camera module is far superior to a USB webcam in terms of image quality. Although, you may already have a webcam knocking about in your box of bits, so why not try it?

## Installing the webcam

After you've connected your webcam to a USB port on your Pi, you can check whether it's been recognized using the `lsusb` command:

```
```bash
$ lsusb
```
```

I'm using a Logitech webcam that gets reported as follows with `lsusb` (Device 006):

```
```text
pi@raspberrypi ~ $ lsusb
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
Bus 001 Device 006: ID 046d:08d8 Logitech, Inc. QuickCam for Notebook Deluxe
```
```

> > Not all webcams will work with the Raspberry Pi. Even though it may be recognized as a USB device, it might not actually work properly with the operating system and create a video device (for example, `/dev/video0`). For example, an old cheap Trust webcam I had appeared as a USB device but wouldn't capture any images. You can check whether your webcam is likely to work with the Pi by checking your make and model at http://elinux.org/RPi_USB_Webcams.

So, now that the Pi knows that we have a webcam device attached, we can use the `fswebcam` utility to capture image frames. You can find out more about fswebcam from the developer's site at http://www.sanslogic.co.uk/fswebcam.

Install `fswebcam` with the following:

```
```bash
$ sudo apt-get install fswebcam
```
```

![](img/631e8b200cce187082ee46b0eb1681b3_385_0.png)

## Taking a snap

You can now test the webcam by capturing a still image, which can be done by running the following command:

```
$ fswebcam test.jpg
```

You should expect to see output similar to the following:

![](img/631e8b200cce187082ee46b0eb1681b3_386_0.png)

> fswebcam has lots of options for things like the resolution and quality of the image. Use the command fswebcam -? to get a list of all options.

## Snap snap snap

fswebcam doesn't take video streams, but you can set it up to take a series of frames at regular intervals. For example, to take a snap every 10 seconds, you can use the following command:

```
$ fswebcam frame.jpg -l 10
```

An example of how this would be useful can be demonstrated by setting the webcam to take a snap every few seconds in the background (the -q switch runs fswebcam in the background). When our security system is triggered, we could then take the latest image snapped with the webcam which could be looking down your pathway.

For the purpose of putting together our entire system later in this book, we'll be focusing on the Raspberry Pi Camera Module, but you can always replace the code with the previous examples if you want to use USB webcams instead.

You'll notice that `fswebcam`, unlike `raspistill`, has the ability to overlay the images with timestamp information, so you don't need to worry about overlaying text as we did previously. Look at the `fswebcam` command line options for more information.

## The multicamera setup

It may have occurred to you that the Raspberry Pi has only one camera module input. Now, this is obviously limiting if you want to have multiple cameras around your property that are triggered by motion detectors.

However, there is nothing stopping us from building standalone units that have a separate Raspberry Pi board with a PIR detector, Camera Module, and network connection, either using a Wi-Fi dongle or Ethernet.

Because you only need a single input to the Raspberry Pi to detect when the PIR motion sensor is triggered, you can use the on-board GPIO port to connect the sensor, rather than using a port expander. The Raspberry Pi will email the alert over the network, and could alert the main controller Pi if required — making it a slave sensor device.

You can readily obtain small PIR detectors, such as the Parallax one shown next, which you can mount onto a Raspberry Pi Case along with the camera module, creating a self-contained unit.

![](img/631e8b200cce187082ee46b0eb1681b3_387_0.png)
