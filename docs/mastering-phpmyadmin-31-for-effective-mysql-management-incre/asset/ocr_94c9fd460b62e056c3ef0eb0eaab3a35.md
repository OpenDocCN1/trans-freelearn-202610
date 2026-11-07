We then click **Save** and are brought back to the **Overview** panel. This save operation did not yet save anything to disk; changes were saved in memory. We are warned that a Blowfish secret key was generated. However, we don't have to remember it, as it's not keyed in during login process, but is used internally. For the curious, you can switch to the **Features** panel and click the **Security** tab to see which secret key was generated. Back to the **Overview** panel. Now our setup process knows about one MySQL server, and there are links that enable us to **Edit** or **Delete** these server settings:

![](bbox=[0.23, 0.28, 0.77, 0.43])

We can have a look at the generated configuration lines by using the **Display** button—then we can analyze these parameters using the explanations given in the *Description of some Configuration Parameters* section later in this chapter.

At this point, this configuration is still just in memory, so we need to save it. This is done via the **Save** button on the **Overview** panel. It saves `config.inc.php` in the special `config` directory that we created previously. This is a directory strictly used for configuration purposes. If, for any reason, it was not possible to create this `config` directory, you just have to **Download** the file and upload it to the web sever directory where phpMyAdmin is installed.

The last step is to copy `config.inc.php` from the `config` directory to the top-level directory – the one that contains `index.php`. By copying this file, it becomes owned by the user instead of the web server, ensuring that further modifications are possible. This copy can be done via FTP or through commands such as:

```
cd config
cp config.inc.php ..
```

> As a security measure, it's recommended that you change the permission on the config directory – for example, with the chmod ugo-rwx config command. This is to block any unauthorized reading and writing in this directory.