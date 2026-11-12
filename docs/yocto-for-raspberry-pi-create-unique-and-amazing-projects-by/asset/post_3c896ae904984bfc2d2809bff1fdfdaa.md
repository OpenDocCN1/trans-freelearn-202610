
## Testing the Nunchuck application

Now, we can launch BitBake and test our program on the Raspberry Pi. Use this command:

```
$ bitbake raspberry-packt-image
```

On the Raspberry Pi, launch the following command:

```
$ nunchuck
```

You can now have fun with Nunchuck and Video4Linux (V4L).

Next, we will see how to integrate video support to our environment.

### V4L presentation

V4L is a video API for Linux. It is integrated into the Linux kernel. It is an abstract layer between video software and video devices. It allows the capture of video streams and pictures from digital camcorders, video capture cards, TV and radio tuners, webcams, and so on.

For example, it is possible to work with OpenCV (to retrieve the video stream through /dev/video) and Qt to display content (using a framebuffer or X11).

## Video support

In order to add video support, we need only look at the readme file of our BSP layer (meta-raspberry-pi). It will be evident that we can easily add video support:

### 2.F. Optional - Video camera support with V4L2 drivers

Set this variable to enable support for the video camera (Linux 3.12.4+ required)

```
VIDEO_CAMERA = "1"
```

The VIDEO_CAMERA variable must be set in conf/local.conf.

### v4l-utils integration

In order to control the device connected to /dev/videoX, there is a set of utilities called v4l-utils. The integration of these tools is easy, as usual, through our build system (Yocto/OpenEmbedded).

To integrate the v4l-utils package to the Raspberry Pi environment, we have to add the following line to raspberry-packt-image:

```
# Base this image on core-image-minimal
include recipes-core/images/core-image-minimal.bb
DESCRIPTION = "Image for raspberry-pi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
# Include modules in rootfs
IMAGE_INSTALL += " \
	kernel-modules \
	gpio-packt \
	i2c-tools \
	spitools \
	nunchuck \
	v4l-utils \
"
```

After that, we just have to launch the following command:

```
$ bitbake raspberry-packt-image
Loading cache: 100%
|############################################################################|
###| ETA: 00:00:00
Loaded 2057 entries from dependency cache.
NOTE: Resolving any missing task queue dependencies
Build Configuration:
BB_VERSION = "1.27.1"
BUILD_SYS = "x86_64-linux"
NATVELSBSTRING = "Ubuntu-14.04"
TARGET_SYS = "arm-poky-linux-gnueabi"
MACHINE = "raspberrypi"
DISTRO = "poky"
DISTRO_VERSION = "1.8+snapshot-20150804"
TUNE_FEATURES = "arm armv6 vfp arm1176jzfs callconvention-hard"
TARGET_FPU = "vfp"
meta
meta-yocto
meta-yocto-bsp = "master:19f77cf586fbee9e67d3698263402b717303c5ec"
meta-raspberrypi = "master:7457bf182c8fd550ec877ecd786a3edd16e65495"
meta-packt_rpi = "master:19f77cf586fbee9e67d3698263402b717303c5ec"
meta-oe
meta-multimedia = "master:f637fadb106a09a6f3dfba4181d06dc9b5e82ff5"
NOTE: Preparing RunQueue
NOTE: Executing SetScene Tasks
NOTE: Executing RunQueue Tasks
Currently 1 running tasks (558 of 566):
0: v4l-utils-1.6.2-r0 do_configure (pid 20717)
```

Once the process is complete, it will be possible to use v4l-utils.

### Note

If you want more information about v4l-utils, you can visit http://www.linux-projects.org/modules/sections/index.php?op=viewarticle&artid=16 .

## Summary

In this chapter, we learned how to integrate a package to test the SPI interface of the Raspberry Pi. We also learned how to create an application for the Wii Nunchuck and how to deploy it inside the Yocto Project.

In the next chapter, we will understand how to integrate a media hub with the Raspberry Pi.

## Chapter 9. Making a Media Hub on the Raspberry Pi

In this chapter, we will learn how to deploy a custom application in order to make an embedded media hub (panel control for temperature, ADC, serial, and so on).

We will work on aspects of rpm packages, init scripts, recipes, and web development.

### Project description – CPU temperature monitoring

The idea of this project is the creation of a web interface that allows, for example, monitoring of the CPU temperature.

To do this, we will use technologies such as **HTML5** and **nodejs**. We will also have utilities available on the Raspberry Pi, such as **vcgencmd**.

In this project, we will use technical aspects, such as:

- Websocket
- Justgage
- Nodejs

The idea here is not to explain these technologies, but to learn that they can be used in the Yocto Project.

### Overview

The following diagram represents the project's structure:

![](img/76681dcfb8cd2bd107d0c06ab088f916_167_0.png)

### Hardware/software requirements

You'll need the following hardware and software for this project:

- A Raspberry Pi
- A host PC
- An SD card
- SSH/SCP

### Creating the main application

The main project contains several files. To best present things, we'll only concentrate on two files:

- server.js: This is the file that will create our web server and send data to the HTML web page
- index.html: This is the page that will open the web browser to display the data received

### server.js

The server.js file creates a server on the listening port 3344 and will be responsible for creating a socket (rpi_temperature) that contains the CPU temperature of Raspberry Pi. The socket function uses, among others, the vcgencmd command ([https://github.com/raspberrypi/firmware/blob/master/opt/vc/bin/vcgencmd](https://github.com/raspberrypi/firmware/blob/master/opt/vc/bin/vcgencmd)), permitting us to monitor the CPU temperature, as shown here:

```
setInterval(function()
{
    child = exec("vcgencmd measure_temp | awk -F:", function
    (error, stdout,
    stderr)
    {
    if (error != null)
    {
    console.log('exec error: ' + error);
    }
    else
    {
    var date = new Date().getTime();
    var temp = parseFloat(stdout);
    socket.emit('rpi_temperature', date, temp);
    }
    }
    }); 1000);
```

### Note

For further information about vcgencmd, you can visit the following page: [http://elinux.org/RPI_vcgencmd_usage](http://elinux.org/RPI_vcgencmd_usage)

### index.html

Here is an example of a function receiving the socket (rpi_temperature) containing the CPU
temperature of our Raspberry Pi:

```
$(document).ready(function(){
    socket.on('rpi_temperature', function (time, data) {
        gg1.refresh($.trim(data));
        return false;
    });
});
```

### Creating the Yocto/OE environment

Now that we have developed the main application, what remains for us is to integrate it with our Yocto RPI environment.

### Modifying the image

The first step consists of integrating the nodejs package (located at meta-openembedded/meta-oe/recipes-devtools/nodejs) with the Raspberry Pi environment. For this, we have to add the following line to raspberry-packt-image:

```
# Base this image on core-image-minimal
include recipes-core/images/core-image-minimal.bb
DESCRIPTION = "Image for raspberry-pi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
# Include modules in rootfs
IMAGE_INSTALL += " \\
 kernel-modules \\
 gpio-packt \\
 i2c-tools \\
 spitools \\
 nunchuck \\
 v4l-utils \\
 nodejs \\
"

```

We have now integrated nodejs with our Poky distro.

### Creating the recipe file

The second step consists of creating the recipe file; let's call this recipe webserver-packt_01.bb.

The idea is to have the following architecture inside the recipes-custom folder:

```
$ tree webserver-packt/
webserver-packt/
├── webserver-packt
│   └── server.tar.gz
└── webserver-packt_01.bb
```

### Note

The server.tar.gz tarball contains the main application (nodejs + HTML5).

To create this recipe, we will use the recipetool command, as follows:

```
$ source oe-init-build-env rpi-build
$ recipetool create -d -o webserver-packt_0.1.bb ../meta-packt_rpi/recipes-custom/webserver-packt/webserver-packt/
$ cp webserver-packt_0.1.bb ../meta-packt_rpi/recipes-custom/webserver-packt/
```

### Note

The complete documentation of recipetool command is online at http://www.yoctoproject.org/docs/1.8/mega-manual/mega-manual.html#new-recipe-creating-the-base-recipe-using-recipetool .

Now we just need to copy the entire archive (server.tar.gz) containing the sources to ${datadir} (/usr/share/); this step is carried out in the do_install() block, as shown here:

```
DESCRIPTION = "Panel to monitor rpi temperature"
LICENSE = "GPLv2"
LIC_FILES_CHKSUM =
"file://${COREBASE}/meta/COPYING.GPLv2;md5=751419260aa954499f7abaabaa882bbe"

# Package Release
PR = "r0"

# Use local tarball
SRC_URI = " \\
file://server.tar.gz \\
"

RDEPENDS_${PN} += "bash"

# Make sure our source directory (for the build) matches the directory structure in
the tarball
S = "${WORKDIR}"

do_install() {
install -d ${D}${datadir}/server-packt
```
