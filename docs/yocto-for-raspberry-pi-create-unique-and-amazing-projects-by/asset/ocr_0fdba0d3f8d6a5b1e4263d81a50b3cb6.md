# # Baking spi-tools

In order to generate the package (ipk, rpm, or dpkg), we can run the following command:

```
$ bitbake spitools
Loading cache: 100%
|###############################################################################|
#################################################################################|
###| ETA:  00:00:00
Loaded 2006 entries from dependency cache.
NOTE: Resolving any missing task queue dependencies
Build Configuration:
BB_VERSION        = "1.27.1"
BUILD_SYS         = "x86_64-linux"
NATIVELSBSTRING   = "Ubuntu-14.04"
TARGET_SYS        = "arm-poky-linux-gnueabi"
MACHINE           = "raspberrypi"
DISTRO            = "poky"
DISTRO_VERSION    = "1.8+snapshot-20150729"
TUNE_FEATURES     = "arm armv6 vfp arm1176jzfs callconvention-hard"
TARGET_FPU        = "vfp"
meta
meta-yocto
meta-yocto-bsp    = "master:19f77cf586fbee9e67d3698263402b717303c5ec"
meta-raspberrypi  = "master:7457bf182c8fd550ec877ecd786a3edd16e65495"
meta-packet_rpi   = "master:19f77cf586fbee9e67d3698263402b717303c5ec"
meta-oe           = "master:f637fadb106a09a6f3dfba4181d06dc9b5e82ff5"
NOTE: Executing SetScene Tasks
NOTE: Executing RunQueue Tasks
Currently 1 running tasks (373 of 381):
0: spitools-git-r0 do_configure (pid 3535)
```

Alternatively, if we want to integrate spi-tools into our image, we just have to add the following line to
our image and run BitBake raspberry-pack-image:

```
# Base this image on core-image-minimal
include recipes-core/images/core-image-minimal.bb
DESCRIPTION = "Image for raspberry-pi"
IMAGE_FEATURES += "ssh-server-dropbear splash"
# Include modules in rootfs
IMAGE_INSTALL += " \
    kernel-modules \
    gpio-pact \
    spitools \
    "
```