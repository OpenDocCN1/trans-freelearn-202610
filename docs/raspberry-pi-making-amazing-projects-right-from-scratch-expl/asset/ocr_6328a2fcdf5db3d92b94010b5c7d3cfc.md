```
```sh
{
    #$1 - GPIO pin number
    #$2 - Value
    sudo echo $2 > /sys/class/gpio/gpio$1/value
}
#Helper functions to switch on and off the outputs
function almSetArmedLED()
{
    #$1 - 0 or 1 (Off or On)
    almSetGPIOValue 23 $1
    echo "[ALM] Armed LED set to $1"
}
function almSetExitBuzzer()
{
    #$1 - 0 or 1 (Off or On)
    almSetGPIOValue 24 $1
    echo "[ALM] Exit Buzzer set to $1"
}
function almSetAlarmLED()
{
    #$1 - 0 or 1 (Off or On)
    almSetGPIOValue 25 $1
    echo "[ALM] Alarm Trigger LED set to $1"
}
function almSetAlarmBell()
{
    #$1 - 0 or 1 (Off or On)
    almSetGPIOValue 4 $1
    echo "[ALM] Alarm Bell set to $1"
}
```
```

And, we'll add a helper function that will read the ARM switch status from the D0 (GPIO17) of the Raspberry Pi and from the web-console to see if the ARM soft switch has been set:

```
```sh
#this function returns whether the system is armed via
#either the web console or key switch
function almGetArmedSwitchStatus()
{
    STAT_RET_VAL="0"
    #read arm key switch input from
    local L_VAL=$(sudo cat /sys/class/gpio/gpio17/value)
    if [ $L_VAL -eq 1 ]; then
        #system has been armed with key switch
```
```