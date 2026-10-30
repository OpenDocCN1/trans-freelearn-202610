
For the home security system in this chapter, I have come up with the following system diagram that we will look to as a framework. The whole concept is designed to be modular, so you can come up with your own system to suit your requirements and implement it accordingly, using the scripts presented in this chapter.

![](img/631e8b200cce187082ee46b0eb1681b3_430_0.png)

The final home security system diagram

## Overview of the system elements

The preceding system diagram comprises the elements and modules that we have discussed in previous chapters. Here's a quick recap of these:

### A +12V power supply

This is the primary power supply to our system, which we will obtain from an external mains adapter that could be **battery-backed**. This supply needs to be smooth and regulated to ensure that it remains stable for the system as currently drawn.

All of the alarm wiring and sensors will be supplied with this power, as will peripherals such as sounders and bells, which usually operate from a 12V supply. *Chapter 5, Adding a Passive Infrared Motion Sensor* discussed the merits of using a 12V supply for the alarm circuits.

### A +3.3V power supply

This supply is a regulated +3.3V supply for the digital port expander circuit; it also provides the logical alarm zone inputs via an opto-coupler. The +3.3V power supply can be derived from either the +12V supply (recommended), or the +5V supply from the Raspberry Pi's GPIO connector, using a voltage regulator chosen according to how much current you need.

*Chapter 3, Extending Your Pi to Connect More Things,* showed you how to build a +3.3V regulated supply.

### The opto-isolator input module

This will isolate the +12V zone input power lines from the port expander and GPIO digital inputs, which should only have a maximum of +3.3V presented to them when triggered high.

The circuit for these opto-isolated input modules was discussed and shown in *Chapter 5, Adding a Passive Infrared Motion Sensor.*

### The port expander

The port expander is our main digital input/output system that will take the alarm zone inputs and transmit them to the Raspberry Pi using the I2C bus, or allow the Raspberry Pi to switch outputs on and off.

We built our MCP23017-based port expander circuit in *Chapter 3, Extending Your Pi to Connect More Things* and configured the software for it in *Chapter 4, Adding a Magnetic Contact Sensor.*

### An arm/disarm switch

The arm/disarm input overrides the arm/disarm **soft-switch** function on our web-based control panel, and is a switch (key, digital keypad, or otherwise) connected to GPIO directly on the Raspberry Pi’s GPIO connector.

Remember to connect any switch circuit appropriately to the GPIO pin to avoid damage to your Raspberry Pi. This was discussed in *Chapter 2, Connecting Things to Your Pi with GPIO*.

### Alarm outputs

In our system, we have several output devices that are controlled by our Raspberry Pi via output driver circuits. We have an output for an entry/exit buzzer, an armed status LED, an alarm bell, and an alarm LED indicator.

These are switched on and off by our Raspberry Pi GPIO connector via driver circuits that allow us to drive high current and inductive loads using the GPIO pins. These driver circuits, based around TIP120 Darlington transistors, were discussed in *Chapter 6, Adding Cameras to Our Security System* and *Chapter 8, A Miscellany of Things*.

## Designing the control scripts

Before we start writing the scripts to control our alarm systems, it is probably a good idea to outline the **high-level** process for the system. The following **flow-chart** helps us picture how our system should work, and the various logical decisions our script needs to make.

The flowchart might look a bit complicated with all its lines in different directions, but it's actually pretty linear and in a downward direction. Referring to the flowchart, it shows the following tasks that the control script will be doing:

- Sitting quietly until the system is armed either by the hardware key switch or the web-based panel's soft switch.
- When the system is first armed, it will sound the exit buzzer for a pre-determined amount of time before actually arming the system. This gives you a chance to leave the property or disarm the system again, before it starts monitoring the inputs.
- Once the system is armed, the armed LED will be switched on and the system will wait to see if any of the alarm zone inputs are triggered. It will also wait to see if the alarm is disarmed on your return to the property. We can optionally put an entry timer in here on the entry zone to delay before triggering the alarm.
- If the alarm is ultimately triggered, then the main alarm bell will be switched on, as well as the exit buzzer. The main bell should only sound for a while, depending on environmental restrictions in your neighborhood, and so, this will be switched off after a pre-defined period, but the internal buzzer will stay on.
- When triggered, the system will then wait for you to disarm it, before resetting it.

![](img/631e8b200cce187082ee46b0eb1681b3_433_0.png)

The control script flowchart

## Building the control script

Now that we have designed our system the way we want it to work, we can start writing our Bash control script. As before, we'll locate our scripts in the folder, /etc/pi-alarm, which, you'll remember from Chapter 7, Building a Web-Based Control Panel, is also where our Web-based control panel writes its configuration status file, alarm.cfg to. We'll be referring to that file in our scripts too.

In this script, we are going to use the bc tool (the Bash command-line calculator) to convert hex values to binary. It's not installed by default, so you'll need to get the package:

```
$ sudo apt-get update
$ sudo apt-get install bc
```

![](img/631e8b200cce187082ee46b0eb1681b3_434_0.png)

> Our script file is quite long so, as before, you might want to sit on the sofa and write it on your laptop using something such as Notepad++. Remember, however, if you're using a PC, ensure that the end-of-line (EOL) format is converted to the Unix format, otherwise the Bash script won't run on the Pi when you copy it across. Notepad++ will do this for you.

## Exploring the script code

I'm now going to walk you through the various sections of the control script code I've written, which will be used as a framework for our system. I say "framework" because, while it will provide you with a fully functional control script for the system, it can be modified and extended to suit your particular requirements.

The following code listings are all part of the single bash script, alarm-control.sh, that can be downloaded in full with comments from the Packt Publishing website.

### Declarations

We'll start off by setting up the various control variables needed to track the system's state:

```
#!/bin/bash
#/etc/pi-alarm/alarm-control.sh

ALM_BELL_DURATION=600    #duration in seconds the alarm bell
should sound for
ALM_EXIT_DELAY=30    #entry/exit zone delay in seconds
```

```
ALM_KEY_ARMED=0      #status of the arm/disarm key switch
ALM_SYS_ARMED=0      #armed status of the system

ALM_ZONE_INPUT_READ=""      #this will store the value of the zone
inputs read
ALM_ZONE_INPUT_STAT="00000000"      #binary representation of the inputs
(b7-b0)
ALM_ZONE_INPUT_PREV=""      #previous zone input status
ALM_ZONE_TRIGGER=0      #this will be set to 1 if one or more zones is
triggered
ALM_ZONES_STAT=(0 0 0 0 0 0 0 0)      #dynamic array of normalised zone
status (z1 to z8 order) - 1 is triggered

STAT_RET_VAL=""      #return value from functions
```

Because we could face the situation whereby a HIGH or a LOW input could represent a triggered zone, depending on its configuration and wiring, I have introduced an array of normalized status flags in the variable, ALM_ZONES_STAT, which will be the definitive state as far as the script is concerned. We'll look at the function that deals with this later.

### Updating config settings

In Chapter 7, Building a Web-Based Control Panel, we introduced the configuration file, alarm.cfg, which stores the system status and configuration for the benefit of the Web-based control panel. This file not only needs to be read by the main control script to get any settings made using the control panel, but also needs to be updated with status values from the main control script so that they can be presented back to the control panel, essentially exchanging data between the two sub-systems.

Therefore, we're going to include a helper function that contains the same code called by the Web page PHP script to update this file from the control panel:

```
#This helper function will update the alarm config
#file with the specified value (alarm.cfg) so that
#the Web panel can know the latest status
function almUpdateConfigSetting()
{
    #$1 - Setting Name
    #$2 - Setting Value
    sudo sed -i "s/^\($1\s*= *\).* /\1$2/" /etc/pi-alarm/alarm.cfg
}
```
