# Making it dynamic

Now that we have the layout code defined for our control panel page, we can insert it in our PHP page so that it can be modified dynamically by the PHP script on the Web server, depending on the status of our home security system.

The PHP script will help us achieve the following basic functions:

+   - Updating the configuration file with the position of the on/off switches for zones
+   - Arming and disarming the system
+   - Telling us which zone has been triggered when an intrusion has been detected

Again, I'm not going to go into detail about how the PHP code works, but hopefully the comments within the code will help you follow what's going on, and also help you modify it if you want to change its behavior.

# Getting a bit of help first

Unless you change some of the PHP configuration, it can be a nightmare trying to work out what's gone wrong if you have a small bug in your code, as basically you are presented with...nothing!

So, before we create and build our PHP page, we'll change a couple of settings in the PHP configuration file to make sure we know if there are any issues:

+   1. Open the configuration file with Nano:
+      `$ sudo nano /etc/php5/cgi/php.ini`
+   2. The file is a bit large and unwieldy, but battle your way through it, find these settings, and change them as follows:
+      `error_reporting = E_ALL`
+      `display_errors = On`
+   3. Save the file and exit Nano.
+   4. Finally, restart Lighty:
+      `$ sudo /etc/init.d/lighttpd restart`