```
ALM_SYS_ARMED=0
almSetAlarmBell 0 #switch off alarm bell
almSetExitBuzzer 0 #switch off exit buzzer
almSetAlarmLED 0
almSetArmedLED 0 #switch off armed LED

########################################
fi

done
##################################################

done
########################################################
```

# We're done (almost)…

And there we have it: a framework for an entire alarm control script on our Raspberry Pi. Additional features that you may want to implement within your script could include the following:

+   - Sending a photo or video clip from a zone's camera when it's triggered
+   - Sending an email alert with status details when the alarm has been triggered
+   - Writing a regular log file recording historical status information
+   - Adding additional environmental sensors to port B

> Each of the script blocks is taken from the single script file, `alarm-control.sh`, so you should be able to put all of the described pieces together into one file to have a fully functional script.

As always, before we can run it we need to give the script execute rights:

```
$ sudo chmod 777 /etc/pi-alarm/alarm-control.sh
```

After we copy the script to our Raspberry Pi, this is what we should see in our `/etc/pi-alarm` folder:

```
pi@raspberrypi ~ $ ls -1 /etc/pi-alarm
alarm.cfg
alarm-control.sh
update-alarm-setting.sh
```