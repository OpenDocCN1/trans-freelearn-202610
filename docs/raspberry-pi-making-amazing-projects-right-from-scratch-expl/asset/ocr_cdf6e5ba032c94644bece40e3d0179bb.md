# Setting Up a Web Server on the Raspberry Pi

Enter all the details for the database including the password that you set earlier. Enter the details on the page as given in the preceding screenshot, and then press Submit. The installation wizard will give an error message that it was unable to write the wp-config.php file to the filesystem. There is no reason to panic here, because it also gives us the content of the wp-config.php file that we can use to create the file manually. The error looks like this:

![](bbox=[0.178, 0.254, 0.822, 0.645])

So, now copy the text given in the dialog box, and open a new terminal. Create a new wp-config.php file in the HTML folder with the following commands:

```
$ cd /var/www/html
$ sudo nano wp-config.php
```