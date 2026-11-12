
fragments, since it takes the kernel configuration directly from the kernel repository (bcmrpi_defconfig or bcm2709_defconfig) and overrides the defconfig file. In order to change kernel configuration options for meta-raspberrypi, a couple of possibilities exist. The first option is to create an integration patch for the kernel that modifies the chosen defconfig file within the kernel tree and is then applied through the kernel recipe, like this:

```
diff --git a/recipes-kernel/linux/linux-raspberrypi_4.1.bb b/recipes-kernel/linux/linux-raspberrypi_4.1.bb
index 3a3cf40..bddf79b 100644
--- a/recipes-kernel/linux/linux-raspberrypi_4.1.bb
+++ b/recipes-kernel/linux/linux-raspberrypi_4.1.bb
@@ -1,6 +1,8 @@
LINUX_VERSION ?= "4.1.10"

SRCREV = "b74df9228c27f55361c065bc5dbfba88861cc771"
-SRC_URI = "git://github.com/raspberrypi/linux.git;protocol=git;branch=rpi-4.1.y"
+SRC_URI = "git://github.com/raspberrypi/linux.git;protocol=git;branch=rpi-4.1.y \
+    file://0001-bcmrpi_defconfig-add-debug.patch \
+    "
```

Another option is to use the kernel_configure_variable function from linux.inc in meta-raspberrypi and then prepend the configuration of the kernel recipe using this function:

```
diff --git a/recipes-kernel/linux/linux.inc b/recipes-kernel/linux/linux.inc
index fae78b7..12eec7e 100644
--- a/recipes-kernel/linux/linux.inc
+++ b/recipes-kernel/linux/linux.inc
@@ -107,6 +107,8 @@ do_configure_prepend() {
        # Activate CONFIG_LEGACY_PTYS
        kernel_configure_variable LEGACY_PTYS y

+       kernel_configure_variable TOUCHSCREEN_STMPE y
+
        # Keep this the last line
        # Remove all modified configs and add the rest to .config
        sed -e "${CONF_SED_SCRIPT}" < '${WORKDIR}/defconfig' > '${B}/.config'
```

## Configuring the kernel with LCD support

When using an LCD display with your Raspberry Pi, your kernel must fulfill some requirements in order to ensure that the display is fully functional. First of all, you need to know whether the kernel you want to use supports your display. Using an old kernel version with a brand new display is often problematic since the drivers required for your display didn't exist when the old kernel was developed, and new drivers are seldom backported to older kernel versions. This leaves you with two choices. The first is doing the hard work of trying to backport the requested drivers by yourself. This is very difficult and often hard to achieve even for an experienced kernel developer, depending on how old kernel is that the backport is needed for. The other, more suitable, solution is to use a newer kernel version with the drivers for your display included.

Even if your kernel contains all the parts needed to get your display up and running, you have to ensure that all drivers are enabled. Starting from kernel version 3.18, Raspberry Pi has official support for something called a device tree, which can be used to simplify the configuration of the kernel.

In this subsection, we will go through these topics:

- The device tree: What is it and how to use it on the Raspberry Pi
- The configuration of a touchscreen from the kernel perspective: How to configure the kernel on the Raspberry Pi to handle different touchscreens

### The Raspberry Pi device tree

A **device tree (DT)** is a tree data structure that contains a description of the device's hardware. A DT can manage resource allocation and handle module loading, which eases problems with drivers competing for system resources. For the ARM architecture, **device tree source (DTS)** files are stored within the kernel tree under `/arch/arm/boot/dts`. When the kernel is build, the DTS files are compiled into **device tree blobs (DTBs)**. To be able to use DTs, both the firmware and kernel must support it, since the bootloader will be responsible for loading the DTB along with the kernel (the third-stage bootloader, `start.elf` will pass the device tree blob to the Raspberry Pi).

### Note

The Raspberry Pi uses its GPU (VideoCore IV) to boot the system. It starts by loading its first-stage bootloader. This will make it possible to access and run the second-stage bootloader, `bootcode.bin` (located on the SD card). The second-stage bootloader is executed on the GPU and will load the third-stage bootloader, called `start.elf`. The main responsibilities of the third-stage bootloader are to read and parse configurations and pass them to the GPU and kernel, together with loading the kernel image. The `bootcode.bin` and `start.elf` bootloaders, together with the configuration (such as `cmdline.txt` and `config.txt`), are located on the SD card.

Passing a DTB to a kernel that doesn't support device trees will prevent the image from booting. In order to handle this in a generic way, a trailer added by the `mkknlimg` tool is used by the bootloader to check whether the kernel supports DT.

The management of device tree is done using the `config.txt` file for the Raspberry Pi. The base device trees are located in the /boot partition alongside start.elf and are named bcm*-rpi-b[-plus].dtb (the A and A+ models are compatible and are supposed to use B and B+ device tree's). An example from config.txt of enabling the i2c interface, adding HiFiBerry support, and ensuring that the base DT for the Raspberry Pi B+ is loaded can be seen here:

```
dtparam=i2c=on
dtoverlay=bcm2708-rpi-b-plus
dtoverlay=hifiberry-dacplus
```

When using the latest firmware, the loader will, based on your hardware, try to load one of the base device trees by default without the need for you to specify it in config.txt.

## Configuring the touchscreen from the kernel perspective

In this subsection, we will go through the basic kernel configuration needed for the PiTFT 2.8 touchscreen and the Raspberry Pi 7 touchscreen.

First, we need to choose what configuration we should use for our base. When using meta-raspberrypi, this will be handled automatically by the build system after we have specified either raspberrypi or raspberrypi2 as our MACHINE variable in local.conf. When using a custom kernel, we need to know which existing configuration is suitable for our Raspberry Pi model. Currently, there are three choices: bcmrpi_defconfig (first-generation Raspberry Pi), bcm2709_defconfig (second-generation Raspberry Pi), or bcm2835_defconfig (only fully supported in later kernel versions). In our example, we will be using a Raspberry Pi 2. When the configuration has been decided, we can start by doing a basic configuration of the kernel, as shown here:

```
$ cd /path/to/rpi_kernel_repo
$ mkdir -p /path/to/output_dir
$ O=/path/to/output_dir CROSS_COMPILE=/path/to/arm-linux-gnueabi- ARCH=bcm2709_defconfig
```

Using BitBake and meta-raspberrypi, you need to do the following:

```
$ bitbake linux-raspberrypi -c configure
```

### Note

For a custom kernel, the config will be stored at /path/to/kernel_output/.config. When using Yocto and meta-raspberrypi, it will be stored at . /tmp/work/raspberrypi*-poky-linux-gnueabi/linux-raspberrypi/<kernel_version_and_revision>/linux-raspberrypi*-standard-build/.config.

Once we have our basic configuration in place, we can check whether it contains the most basic configurations required for bringing up our display. Note that the following configurations are currently enabled by default in the Raspberry Pi configurations.

For PiTFT 2.8, these options must be enabled:

```
CONFIG_INPUT_TOUCHSCREEN=y
CONFIG_OF_TOUCHSCREEN=y
CONFIG_TOUCHSCREEN_STMPE=m
CONFIG_FB_TFT_FBTFT_DEVICE=m
```

Besides the generic touchscreen support configuration, we need to ensure that the stmpe and fbtft devices are built. The stmpe_ts device is needed to get working touch functionality for the PiTFT. The fbtft device is a framebuffer driver for small TFT LCD displays, such as the PiTFT's. The device supports the use of SPI and GPIO, which are used by PiTFT displays, and will ensure that the display is registered and noticed by the kernel. Besides these options, basic setup of SPI and i2c is required to get everything up and running.

In this book, we will assume the usage of a kernel version with device tree support for Raspberry Pi (kernel 3.18+). This means that we need to modify config.txt to make sure that required parts are automatically loaded using device trees, like this:

```
dtparam=spi=on
dtparam=i2c1=on
dtparam=i2c_arm=on
dtoverlay=pitft28r, rotate=90, speed=32000000, fps=20
```

For the official Raspberry Pi 7, it's enough to enable the following:

```
CONFIG_INPUT_TOUCHSCREEN=y
CONFIG_OF_TOUCHSCREEN=y
CONFIG_TOUCHSCREEN_RPI_FT5406=m
```

Then, enable ft5406 overlay in config.txt:

```
dtoverlay=rpi-ft5406
```

We have learned what basic configuration is needed for the kernel to be functional with some common touchscreen models for the Raspberry Pi. In the next section, we will learn how these parts, together with required userspace settings, can be set up using the Yocto Project.
