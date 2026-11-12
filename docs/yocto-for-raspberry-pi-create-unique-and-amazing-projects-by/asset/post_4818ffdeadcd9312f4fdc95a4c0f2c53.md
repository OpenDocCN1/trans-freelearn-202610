
## Raspberry-Pi Remote lighting control

### The look of the client (index.html) when displayed in a web browser

Lastly, we must not forget to add gpio-packt and webserver-packt to our image:

```
$ cat recipes-core/images/packt-iot-image.bb
# Base this image on rpi-basic-image
include recipes-core/images/rpi-basic-image.bb
SPLASH = "psplash-raspberrypi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
IMAGE_INSTALL_append = " rpi-gpio gpio-packt webserver-packt"
```

## Creating a package list for your image

One of the reasons you would want to create a customized image is that you often require a different set of packages than those used in existing images. When using the Yocto Project, it's possible to use a sort of package list, to make it easier for the end user to add new packages and to keep the image recipe as simple as possible. This section will guide you through how to use a customized package list using the Yocto Project.

## Setting up a customized package list

First, you need to add a new BitBake file (.bb) named packagegroup-packt-iot.bb:

```
$ mkdir recipes-core/images/packagegroups
$ touch packagegroup-packt-iot.bb
```

A typical package list will look something like this:

```
$ cat packagegroup-packt-iot.bb
DESCRIPTION = "Package list for packt-iot-image"
LICENSE = "MIT"
inherit packagegroup
RDEPENDS_${PN} += " \\
pkg1 \\
pkg2 \\
pkg3 \\
"
```

## Start using a customized package list in meta-packt-iot

When our customized package list is in place, we can transfer packages added through IMAGE_INSTALL. This will make the package group look like the following:

```
$ cat packagegroup-packt-iot.bb
DESCRIPTION = "Package list for packt-iot-image"
LICENSE = "MIT"
inherit packagegroup
RDEPENDS_${PN} += " \\
rpi-gpio \\
nodejs \\
gpio-packt \\
webserver-packt \\
"
```

Finally, we need to ensure that our new package list is used; for this example, it will be done by replacing the content in IMAGE_INSTALL with the package group recipe, like this:

```
# Base this image on rpi-basic-image
include recipes-core/images/rpi-basic-image.bb
SPLASH = "psplash-raspberrypi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
IMAGE_INSTALL_append = " packagegroup-packt-iot"
```

### Note

The Yocto Project documentation contains the following information about IMAGE_INSTALL: "Using IMAGE_INSTALL with the += operator from the /conf/local.conf file or from within an image recipe is not recommended as it can cause ordering issues. Since core-image.bbclass sets IMAGE_INSTALL to a default value using the ?= operator, using a += operation against IMAGE_INSTALL will result in unexpected behavior when used in conf/local.conf. Furthermore, the same operation from within an image recipe may or may not succeed depending on the specific situation. In both these cases, the behavior is contrary to how most users expect the += operator to work." For more details, visit http://www.yoctoproject.org/docs/current/ref-manual/ref-manual.html#var-IMAGE_INSTALL.

## Putting it all together

At this point, we have a basic idea about how to build our project, and we also have all the software required for our project. In this section, we will go through a number of iterations until we reach the final goal of using a remote device to control the lamp.

We will do this in three steps:

- 1. Control the relay using the Raspberry Pi.
- 2. Control the lamp using the Raspberry Pi.
- 3. Turn on/off the lamp from a smartphone.

During development, it's inefficient to always boot the Raspberry Pi using, for example, an HDMI connection to a TV that also requires a separate mouse and keyboard to control the Pi. Using HDMI, we will not be able to track the early boot procedure either. For this project, we also want to be able to troubleshoot the Raspberry Pi without the need to place it close to a TV. For our project, we will either use SSH or a serial connection to reduce turnaround time during development.

## Serial and SSH connections to the Raspberry Pi

The first method is to use a serial connection to the Raspberry Pi. The easiest way to achieve this is to use a USB-to-serial cable. Connect the USB to your host machine. Then, connect the four wire cables to the correct GPIO pins on the Raspberry Pi, as shown in the following figure:

![](img/76681dcfb8cd2bd107d0c06ab088f916_242_0.png)

A USB-to-serial cable connected to the Raspberry Pi GPIO header

The following table will give a more detailed explanation of how the GPIO wires should be connected. Also check the GPIO header diagram in Chapter 8, Diving into the Raspberry Pis Peripherals and Yocto Recipes, to review the purpose of all the pins in the GPIO header.

| Wire | GPIO/Pin |
|---|---|
| Red | 5V |
| Black | GND |
| White | GPIO 14 (TXD) |
| Green | GPIO 15 (RXD) |

Once the physical connection has been set up, we can continue with preparing our host environment. In this example, we will use the screen package to set up our console connection.

To set up screen on Ubuntu, use this command:

```
$ sudo apt-get install screen
```

Use this one to set it up on Fedora:

```
$ sudo yum install screen
```

Next, we must ensure that we have plugged in the USB cable to our Raspberry Pi (remember that no additional power supply is needed). After that, we can open a terminal window and type the following command, which will connect us to the Raspberry Pi console:

```
$ sudo screen /dev/ttyUSB0 115200

<snip>
[   2.017380] [vc_sm_connected_init]: end - returning 0
[   2.023899] uart-pl011 3f201000.uart: no DMA platform data
[   2.034527] EXT4-fs (mmcblk0p2): INFO: recovery required on readonly filesystem
[   2.041844] EXT4-fs (mmcblk0p2): write access will be enabled during recovery
[   2.093584] EXT4-fs (mmcblk0p2): recovery complete
[   2.103945] EXT4-fs (mmcblk0p2): mounted filesystem with ordered data mode.
Opts: (null)
[   2.112154] VFS: Mounted root (ext4 filesystem) readonly on device 179:2.
[   2.122995] devtmpfs: mounted
.
.
.
Starting syslogd/klogd: done
Poky (Yocto Project Reference Distro) 2.0+snapshot-20160229 raspberrypi2
/dev/ttyAMA0
raspberrypi2 login: [   17.906610] random: nonblocking pool is initialized
Poky (Yocto Project Reference Distro) 2.0+snapshot-20160229 raspberrypi2
/dev/ttyAMA0
raspberrypi2 login:
```

If we have connected a network cable to the Raspberry Pi, we can use this method to grab the IP address of our board:

```
root@raspberrypi2:~# ifconfig eth0 | grep "inet addr"
```

```
inet addr:192.168.1.13  Bcast:192.168.1.255  Mask:255.255.255.0
```

The other method to gain quick access to the Raspberry Pi without using any external equipment (such as a mouse, keyboard, or TV) are using SSH. If you have an SSH daemon running on your Raspberry Pi and it's connected to the network, you can access it through SSH using your host machine, with a command similar to this:

```
$ ssh root@192.168.1.13
root@raspberrypi:~#
```

Now we are ready to get going with connecting the relay and lamp to our Raspberry Pi.

## Controlling the relay using the Raspberry Pi

To start with, we will control only the relay from the Raspberry Pi connected to it. This will be done using GPIO pins. First, we need to connect the relay to our Raspberry Pi in the correct way. The following table shows the connection scheme:

| Wire (color) | GPIO pin | Relay pin |
| :--- | :--- | :--- |
| Red | 3.3V (pin 1) | Power/VCC (+) |
| Black | GND (pin 6) | GND (-) |
| Yellow | GPIO 17 (pin 11) | IN (Signal/S) |

When this scheme is physically applied, it will look like the following figure. Remember that the exact order of the relay pins might look different between different models.

![](img/76681dcfb8cd2bd107d0c06ab088f916_246_0.png)

A Raspberry Pi 2 connected to a one-channel relay module, using GPIO 17 for the signaling

When the connection to the relay is complete, we can continue with next step: turning on or off the small LED connected to the relay. Most relays have a small LED attached to them. If your relay doesn't have an LED indicator, don't worry; we will look at some methods of checking from the command line whether the GPIO pin is active or not. There exist a number of different methods to control the GPIO pins on the Raspberry Pi; in this book, we will learn how to use sysfs and RPi.GPIO to control the GPIO pins.

We will start by learning how to use sysfs to turn the LED on and off. First, we need to export the GPIO pins and thereby make them visible from the userspace:

```
$ echo 17 > /sys/class/gpio/export
```
