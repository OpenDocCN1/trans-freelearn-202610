
The advantages are as follows:

- Their ease and speed of installation
- Their ease of removal, which means that you can take them anywhere with you
- Expanding the system in the future can be easier, with most systems automatically detecting new units

The disadvantages are as follows:

- They are more expensive than wired systems, sometimes three or four times the cost
- They are not as secure as wired systems and cannot achieve a security grading greater than two in accordance with European Standard BSEN 50131 (although, this grade is suitable for domestic properties)
- Wireless devices need to have their batteries replaced at regular intervals
- Wireless systems are less reliable and susceptible to interference and even radio jamming

### Connecting a 433-MHz receiver

In the past, it was possible to roll out your own 433-MHz receiver for the Raspberry Pi using an inexpensive receiver, such as the XY-MV-5V module along with the 433-Util library that was put together by a guy called Mark Wolfe, a contributor on GitHub. Essentially, he gathered together code relating to 433-MHz communications and put it all into this library. Originally developed for Arduino, this has now been ported to the Raspberry Pi.

You can then use a readily available transmitter, such as a key fob or any other 433-MHz transmitter, and take a look at the incoming code as you press each button on the transmitter.

![](img/631e8b200cce187082ee46b0eb1681b3_362_0.png)

A XY-MK-5V Generic 433-MHz receiver module

Finding a suitable 433-MHz receiver should be easy as websites such as Amazon and eBay are awash with them, and they cost as little as a couple of pounds.

Note that the 433-MHz band is free for many types of devices. As such, there are various different types of receiver, and although they may all state that they are 433-MHz receivers, they can operate using AM or FM, and some only detect certain types of data. Some, such as the Quasar QAM range, may also require special decoder chips in order to read transmitted data and may only work with paired transmitters.

The receiver module can pick up signals from a key fob remote control, such as the one shown in the following image (this can be picked up from the home security section of any local DIY store), which gets an output as a series of square waves. These square waves are then decoded by the 433-Util software.

![](img/631e8b200cce187082ee46b0eb1681b3_363_0.png)

A Novar/Blyss 433MHz wireless remote control

I liked this particular remote control because I thought it would be good as the **arm** and **disarm** device for our home security system. I will talk about arming and disarming in *Chapter 8, A Miscellany of Things*, where we will look at the ways to achieve this.

The fact that it just switches a relay on and off means that we can easily implement this in our home security system because it simply acts as a switch. When you press the button on the transmitter, the relay switches the contacts on; press it again, and the relay switches off. The screw terminals on the board provide us with access to the relay terminals.

### The receiver wiring diagram

As we are just dealing with a switch input, we can use the same circuit as we did with the zone circuit earlier but connected to our arm/disarm GPIO input, which we'll determine in Chapter 9, Putting It All Together.

![](img/631e8b200cce187082ee46b0eb1681b3_365_0.png)

Circuit to interface the receiver module with a GPIO input

When the receiver module switches the relay on, this will complete the 12V circuit through the opto-coupler's LED by turning it on. This will make the transistor pull the GPIO pin down to ground, giving it a low input.

You can use this type of circuit for any paired receiver for the wireless security devices that you want to use in your system.

### Logging detection data

With any system, it's useful to be able to log data when something happens. We can do this with our detectors too by writing to a log file every time a detector in a zone is triggered. This way, you can keep a log of every time someone enters a room, which you can review at a later date even if the system isn't armed. You can also keep a log of when the system is armed and disarmed.

Here's a simple script that shows you how to do this whenever an event happens on our zones connected to the GPIO inputs:

```
#!/bin/bash

#set up the I2C expansion port
sudo i2cset -y 1 0x20 0x00 0xFF

#reset status
CURR_STATE="0x00"
LAST_STATE="0x00"

#path to the log file
LOG_FILE="/etc/pi-alarm/zones.log"

# loop forever
while true
do
  # read the gpio inputs
  CURR_STATE=$(sudo i2cget -y 1 0x20 0x12)

  #check if state has changed
  if [ "$CURR_STATE" != "$LAST_STATE" ]
  then
    #write change to log file
    TIMESTAMP=`date "+%Y-%m-%d %H:%M:%S"`
    echo "$TIMESTAMP Zone Status Changed from $LAST_STATE to $CURR_STATE" > $LOG_FILE
  fi
  $LAST_STATE = $CURR_STATE
  sleep 1
done
```

The preceding example is quite simple, but it can be made more useful by actually writing out the zone or zones that change by decoding the hex value that's returned by the i2cget command in the constituent zones.

In *Chapter 9, Putting It All Together*, you'll learn how this is done in order to display the individual status of each zone on a web page. You can use exactly the same technique to do this for your log files and, in fact, output to the log file by expanding on the same script.

### Summary

In this chapter, we started off by learning how passive infrared sensors are used to detect motion to protect a predefined coverage area from intrusion. We then looked at connecting these to the inputs on our port expander via opto-couplers as we will now use 12V to power the alarm zone circuits.

We then looked at wireless alarm systems that operate on the open 433-MHz band, which is commonly used for security devices. After exploring the possibility of using the legacy 433-Util bit-banging software on our Raspberry Pi to decode the signals transmitted by devices using a simple receiver, we opted to use a paired receiver device that will interface easily with our alarm circuit inputs.

Finally, we created a simple script that will log the changes in our alarm inputs to a text file, which can later be expanded to log exactly what's going on with the system in detail.

## 6 Adding Cameras to Our Security System

Until now, we've been putting together the elements that will allow us to connect sensors to our alarm system to detect intrusions using either switches or passive infra-red motion detectors, which in turn will tell our Raspberry Pi that something has happened in a particular zone. These elements will all come together as a whole system later in this book.

Our system is now going to become a whole lot more sophisticated with the addition of cameras to take pictures and video clips, and e-mail them to us straightaway when it detects something.

We'll also use e-mail to send us alerts on our smart phone when we're out and about when any of the sensors in the system are triggered.

In this chapter we will cover the following topics:

- Setting up the Raspberry Pi camera module and learning how to capture stills and video images
- Learning how to overlay captured images with text and time-stamps
- Triggering image captures with a motion detector
- E-mailing the image and video files to us in real time
- Understanding the differences between capturing images during the day and during the night
- Switching on and off security lighting and other high-current devices when required
- Connecting a USB webcam instead of the native camera module

### Prerequisites

You'll need the following parts for this chapter, on top of the components used in the previous chapter:

- A Raspberry Pi standard camera module
- A Raspberry Pi NoIR camera module
- An Infra-Red LED array and/or visible LED array
- A USB webcam

### The Raspberry Pi camera module

The Raspberry Pi Camera Module is an official Raspberry Pi accessory that works with all models of the Pi, and can be used to take high-definition stills and video images. It connects directly to the Pi board's camera serial interface (CSI) port, which is dedicated to these modules to enable high-speed operation.

The camera itself is a 5 megapixel fixed-focus sensor supporting 1080p, 720p, and VGA video modes and still captures.

![](img/631e8b200cce187082ee46b0eb1681b3_370_0.png)

The official Raspberry Pi Camera Module
