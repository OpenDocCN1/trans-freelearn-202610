We notice three changes:

+   - The username field cannot be typed in
+   - A small key icon is displayed
+   - An error message appears

It's normal that the username field is deactivated, as user names are now taken from the Swekey configuration file. The icon can be clicked to reach the vendor's website. There is a testing mechanism on this site, to verify if a Swekey is connected. Drivers can also be downloaded in case the automatic installation did not work.

Now if you connect your Swekey, assuming that user marc has been defined and associated to this device, a reassuring panel is displayed:

![](bbox=[0.235, 0.359, 0.765, 0.488])

# Security note

It would not make sense to rely on the protection offered by this device, if our users have access to a webspace on which they can install their own copy of phpMyAdmin. Obviously, phpMyAdmin has to be configured accordingly to make use of the Swekey. Hence, a user-installed copy of phpMyAdmin would circumvent the protection. This means that a host provider should permit access to his MySQL server, only from a web server on which he/she installed a controlled copy of phpMyAdmin, and on which other users cannot install their own copy.

# Summary

This chapter gives us an overview of how to use a single copy of phpMyAdmin to manage multiple servers, and also the usage of authentication types to fulfill the needs of a users' group while protecting authentication credentials. The chapter also covers the ways of securing our phpMyAdmin installation.

We'll now have a look at all the panels and windows that comprise the user interface of phpMyAdmin.