# Logging detection data

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