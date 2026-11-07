> > If this parameter is set to **false** and a login attempt is made with the **root** account and no password, an **Access denied** message is displayed.

# Single-user authentication using config

For our first example, we will use the config authentication type, which is easy to understand. However, in the *Multi-user authentication* section, we will see more powerful and versatile ways of authenticating.

> > Using the **config** authentication type leaves our phpMyAdmin open to intrusion, unless we protect it as explained in the *Security* section of this chapter.

Here we ask for config authentication, and enter our username and password for this MySQL server:

```
$cfg['Servers'][$i]['auth_type'] = 'config';
$cfg['Servers'][$i]['user']      = 'marc';
$cfg['Servers'][$i]['password']  = 'bingo';
```

We can then save the changes we made in config.inc.php.

# Testing the connection

Now it's time to start phpMyAdmin and try connecting with the values we configured. This will test the following:

+   - The values we entered in the config file or on the web-based setup
+   - The setup of the PHP component inside the web server, if we did a manual configuration
+   - Communication between web and MySQL servers

We start our browser and point it to the directory where we installed phpMyAdmin, as in http://www.mydomain.com/phpMyAdmin. If this does not work, we try http://www.mydomain.com/phpMyAdmin/index.php. (This would mean that our web server is not configured to interpret index.php as the default starting document.)

If you still get an error, refer to Appendix B for troubleshooting and support. We should now see phpMyAdmin's homepage. Chapter 3 gives an overview of the panels seen now.