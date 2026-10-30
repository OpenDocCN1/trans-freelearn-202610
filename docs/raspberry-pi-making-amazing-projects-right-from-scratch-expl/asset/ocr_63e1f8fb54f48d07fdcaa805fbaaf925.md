```
# loop forever
while true
do
    # read the sensor state
    SWITCH=$(sudo i2cget -y 1 0x20 0x12)

    if [ $SWITCH == "0x01" ]
    then
        #switch not pushed so turn off LED pin
        sudo echo 0 > /sys/class/gpio/gpio17/value
    else
        #switch was pushed so turn on LED pin
        sudo echo 1 > /sys/class/gpio/gpio17/value
    fi
    #short delay
    sleep 0.5
done
```

> Later, as we add more sensors to different input pins, we will need to be able to detect which one has been triggered. We'll look at writing a Bash function later in the book, which will parse the returned hex value from the i2cget command, and tell us exactly which of the 8 inputs is high.

# Anti-tamper circuits

If you take a closer look at our system, you might realize that depending on whether you are detecting normally open or normally closed sensor switches, it is possible to tamper with the sensor channel by simply cutting the wire. So, in the case of a normally open switch, it wouldn't activate the monitoring system if the wires were cut, as it would always appear to be open, even if the switch was closed.

To mitigate this, most alarm systems feature a 4-core wiring system to connect the sensor devices to the main control board — two cores are used to connect the sensor and two are used to create an anti-tamper loop, which then itself forms a sensor input for monitoring.