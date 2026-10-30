# Adding a Magnetic Contact Sensor

+   4. Select <yes>.
+   5. Reboot your Raspberry Pi for the setting to take effect.

Now that the I2C bus has been enabled, we need to set up the operating system so that the required modules are loaded each time the system boots. To do this, perform the following steps:

+   1. Edit the **Modules file** using the following line:
+      ```bash
+      $ sudo nano /etc/modules
+      ```
+   2. Add the following lines to the file:
+      ```text
+      i2c-bcm2708
+      i2c-dev
+      ```
+   3. Save the file and exit Nano.

# Installing the I2C tools package

So that we can easily access the I2C bus using Bash scripts, we need to install the `i2c-tools` package:

```
```bash
$ sudo apt-get install i2c-tools
```
```

Once installed, we should shutdown our system:

```
```bash
sudo shutdown -h now
```
```

After activity has stopped, switch off your Raspberry Pi, connect your port expander to the GPIO port, and power it back up so that we can start using it.

As a quick sanity check, you can see if I2C support has been loaded by typing:

```
```bash
$ ls /dev/i2c-*
```
```

This should give you a list of at least one bus—for example, `/dev/i2c-1`—if the module is loaded. If it's not, you'll probably get the following response:

```
```text
ls: cannot access /dev/i2c-*: No such file or directory
```
```

> In this case, you'll need to check back through the previous steps as something hasn't happened properly.