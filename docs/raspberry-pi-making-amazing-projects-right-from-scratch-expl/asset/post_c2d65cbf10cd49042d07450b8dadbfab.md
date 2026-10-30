
width: 50%;
height: 30px;
padding: 0;
line-height: 30px;
font-size: 14px;
color: white;
font-family: Trebuchet, Arial, sans-serif;
font-weight: bold;
box-sizing: border-box;
}

.onoffswitch-inner:before {
    content: "ON";
    padding-left: 10px;
    background-color: #34C290;
    color: #FFFFFF;
}

.onoffswitch-inner:after {
    content: "OFF";
    padding-right: 10px;
    background-color: #EEEEEE;
    color: #999999;
    text-align: right;
}

.onoffswitch-switch {
    display: block;
    width: 18px;
    margin: 6px;
    background: #FFFFFF;
    position: absolute;
    top: 0;
    bottom: 0;
    right: 56px;
    border: 2px solid #FFFFFF;
    border-radius: 20px;
    transition: all 0.3s ease-in 0s;
}

.onoffswitch-checkbox:checked + .onoffswitch-label .onoffswitch-inner {
    margin-left: 0;
}

.onoffswitch-checkbox:checked + .onoffswitch-label .onoffswitch-switch {
  right: 0px;
}

.masterswitch {
  position: relative;
  width: 90px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}

.masterswitch-checkbox {
  display: none;
}

.masterswitch-label {
  display: block;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid #FFFFFF;
  border-radius: 20px;
}

.masterswitch-inner {
  display: block;
  width: 200%;
  margin-left: -100%;
  transition: margin 0.3s ease-in 0s;
}

.masterswitch-inner:before, .masterswitch-inner:after {
  display: block;
  float: left;
  width: 50%;
  height: 30px;
  padding: 0;
  line-height: 30px;
  font-size: 12px;
  color: white;
  font-family: Trebuchet, Arial, sans-serif;
  font-weight: bold;
  box-sizing: border-box;
}

```css
}

.masterswitch-inner:before {
  content: "ARMED";
  padding-left: 10px;
  background-color: #F00000;
  color: #FFFFFF;
}

.masterswitch-inner:after {
  content: "OFF";
  padding-right: 10px;
  background-color: #EEEEEE;
  color: #999999;
  text-align: right;
}

.masterswitch-switch {
  display: block;
  width: 18px;
  margin: 6px;
  background: #FFFFFF;
  position: absolute;
  top: 0;
  bottom: 0;
  right: 56px;
  border: 2px solid #FFFFFF;
  border-radius: 20px;
  transition: all 0.3s ease-in 0s;
}

.masterswitch-checkbox:checked + .masterswitch-label .masterswitch-inner {
  margin-left: 0;
}

.masterswitch-checkbox:checked + .masterswitch-label .masterswitch-switch {
  right: 0px;
}
/* END OF SWITCH STYLES */
```

# Building a Web-Based Control Panel

Web control panel style sheet - `alarm-panel.css`

Apply the stylesheet and this is what you end up with (a little bit nicer, I think you'll agree):

![](img/631e8b200cce187082ee46b0eb1681b3_404_0.png)

The web control panel with styling

# Making it dynamic

Now that we have the layout code defined for our control panel page, we can insert it in our PHP page so that it can be modified dynamically by the PHP script on the Web server, depending on the status of our home security system.

The PHP script will help us achieve the following basic functions:

- Updating the configuration file with the position of the on/off switches for zones
- Arming and disarming the system
- Telling us which zone has been triggered when an intrusion has been detected

Again, I'm not going to go into detail about how the PHP code works, but hopefully the comments within the code will help you follow what's going on, and also help you modify it if you want to change its behavior.

# Getting a bit of help first

Unless you change some of the PHP configuration, it can be a nightmare trying to work out what's gone wrong if you have a small bug in your code, as basically you are presented with...nothing!

So, before we create and build our PHP page, we'll change a couple of settings in the PHP configuration file to make sure we know if there are any issues:

- 1. Open the configuration file with Nano:
  `$ sudo nano /etc/php5/cgi/php.ini`
- 2. The file is a bit large and unwieldy, but battle your way through it, find these settings, and change them as follows:
  `error_reporting = E_ALL`
  `display_errors = On`
- 3. Save the file and exit Nano.
- 4. Finally, restart Lighty:
  `$ sudo /etc/init.d/lighttpd restart`

# The main PHP code

And here it is... But don't run it yet—there's still a bit more to do...

You can find the full main PHP code in the `index.php` file located inside the code folder of `chapter 7`. In our Web server content folder, we should now have the following files:

```
pi@raspberrypi ~ $ ls -l /var/www
alarm-panel.css
alarm-panel.html
index.lighttpd.html
index.php
phpinfo.php
```

# I'm someone else

Now, before we can actually open this PHP web page successfully, we need to be aware of the fact that the Web server, by default, actually runs as a different user called `www-data`. This means that it doesn't ordinarily have the right to perform certain operations; in particular, those that interact with the file system.

If you worked through the previous PHP script, you'll see that it actually executes some Linux commands to read and update our `alarm.cfg` file.

In the same way that we have to put `sudo` in front of many commands because we're not the root user, it is true for other users as well, including `www-data`. So, to give the Web server rights to execute certain commands, we need to add it as a **sudoer**, using the **visudo** utility.

Run the utility to open the sudoer configuration file:

```
$ sudo visudo
```

At the bottom of the file, add the following line:

```
www-data ALL=(ALL) NOPASSWD:/bin/cat,/etc/pi-alarm/update-alarm-setting.sh
```

Then save the file and exit.

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

By changing the switch positions and then clicking on the **Update System** button, you should find that the setting values get updated accordingly in `alarm.cfg`. You can now see how this file will be the way for the status to be exchanged between our web console and the security system scripts that we’ll develop in *Chapter 9, Putting It All Together.*

![](img/631e8b200cce187082ee46b0eb1681b3_408_0.png)

The final operational control panel

# Remote access to our control panel

While we can set up our system to receive email alerts when our system detects an intrusion, it would be really useful to be able to access our Web-based control panel wherever we are so that we can perhaps arm and disarm the system or switch off certain zones when we’re not there.
