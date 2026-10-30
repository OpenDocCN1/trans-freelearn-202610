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