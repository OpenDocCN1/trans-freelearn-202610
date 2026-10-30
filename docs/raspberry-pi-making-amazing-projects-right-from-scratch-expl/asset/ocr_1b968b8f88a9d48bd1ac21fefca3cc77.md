# Adding Cameras to Our Security System

As you can see in the following image, the ribbon connector is not that long, so the camera needs to be located close to the Raspberry Pi. By using a camera enclosure, you could actually mount the camera directly on top of the Raspberry Pi case itself, if that works for you.

![](bbox=[0.178, 0.224, 0.822, 0.503])

The camera module, housed within an enclosure

# Setting up the camera module

Before we can use the camera module, we need to enable camera support on the Raspberry Pi. To do this, we use the `raspi-config` tool, as we did with the I2C bus earlier in our journey.

+   1. Connect to your Raspberry Pi the lazy way from your sofa using SSH, or directly using a keyboard and monitor.
+   2. Once you've logged in, launch the config tool with the following command:

```
```bash
$ sudo raspi-config
```
```