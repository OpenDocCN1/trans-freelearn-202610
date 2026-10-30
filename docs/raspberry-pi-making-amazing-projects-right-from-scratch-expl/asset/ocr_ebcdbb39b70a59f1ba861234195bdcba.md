The final thing we have to do is create a small **Bash script** that will handle the task of updating settings in our `alarm.cfg` file. The reason why we need to do this is because we're going to use the Linux sed command to update the file. The way that we are invoking the sed command means that it needs to create a temporary file. Unless we do a bit of work with configuring the Web server because of its file location context, it won't work. So, it's easier to create a stub Bash script that is called by the PHP script. In this way, the Bash environment deals with the temporary file context.

So, we'll create the following Bash script and save it in our `/etc/pi-alarm` folder:

```
```bash
#!/bin/bash
#/etc/pi-alarm/update-alarm-setting.sh
###############################################
# Provides access to the sed command from  #
# PHP as it needs write access to a temp  #
# folder.                                  #
# $1 - Setting Name                        #
# $2 - Setting Value                       #
###############################################

sed -i "s/^ ($1\s*= *).*/\1$2/" /etc/pi-alarm/alarm.cfg
```
```

`update-alarm-setting.sh`

And then we need to give the script execution rights:

```
```bash
$ sudo chmod 777 /etc/pi-alarm/update-alarm-setting.sh
```
```

This is what we should see in our `/etc/pi-alarm` folder at this time:

```
```text
pi@raspberrypi ~ $ ls -l /etc/pi-alarm
alarm.cfg
update-alarm-setting.sh
```
```

Right, after all that, I think we can now launch the control panel page in our browser at

```
```text
http://<my-pi-ip>.
```
```

`index.php` is configured as a default page in Lighty's config, so you don't need to add it to the end of the URL; just the IP address will suffice.