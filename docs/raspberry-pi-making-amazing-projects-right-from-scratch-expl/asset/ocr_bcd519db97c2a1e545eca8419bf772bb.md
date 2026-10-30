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

# Updating config settings

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