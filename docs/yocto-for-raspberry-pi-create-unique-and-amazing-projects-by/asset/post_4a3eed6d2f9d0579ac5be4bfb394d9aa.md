
## The Linux kernel

The Linux kernel is a Unix-like operating system kernel created by Linus Torvalds in 1991. It was originally developed for 32-bit x86-based PCs, but today, Linux also supports a large set of architectures such as ARM, PowerPC, MIPS, and SPARC. The Linux kernel is a fundamental part of a Linux distribution (such as Ubuntu, Debian, and Fedora).

When deciding which kernel is most suited to your project, it's good to have in mind how kernel releases work. Depending on your needs, you can choose between **mainline**, **developing**, or **stable** kernels. If you don't work with kernel development, it's recommended to avoid using the mainline kernel since it's still under development and is considered unstable. A kernel is always considered stable directly after it has been released from the mainline tree. After that, the kernel will be maintained until the next mainline kernel becomes available (usually in 2-3 months). However, a kernel can be picked as a longterm kernel, which means that it will be maintained for an extended period (several years). Longterm kernels are often used in commercial projects, since these projects require a stable and supported base that can have important bug and security fixes backported for a long time.

### Note

Visit [https://www.kernel.org/releases.html](https://www.kernel.org/releases.html) for more details about kernel releases.

## The Linux kernels versus the Raspberry Pi Linux kernel

The currently recommended kernel for the Raspberry Pi is the official Raspberry Pi Linux kernel (https://github.com/raspberrypi/linux.git). The reason we need a specific kernel for the Raspberry Pi (instead of using the mainline Linux kernel) is mainly the proprietary firmware/GPU binary blob needed by the Raspberry Pi. However, in 2014, the full documentation for the VideoCore IV graphics core, together with the complete source for the graphics stack, was released by Broadcom, under a three-clause BSD license. This lead to the gap between the mainline support for bcm2835 (the SoC used in the Raspberry Pi) and the support for the Raspberry Pi Linux kernel decreasing significantly. With that said, there is still some hard work left to do in the mainline kernel, for example, video decoding and camera support. Also, some important drivers such as the rpi-ft5406 (used for the official Raspberry Pi touchscreen) still don't exist within the mainline Linux kernel.

### Note

Read more about the current status of the Raspberry Pi kernel upstreaming to the mainline kernel at http://elinux.org/RPi_Upstreaming.

## Getting started with the Linux kernel

In this part of the chapter, we will go through how to download, configure, and compile a custom Linux kernel.

Different mainline versions of the kernel can be downloaded from kernel.org, either as an archived file or through GitHub. In the examples, we will use version 4.1 of the kernel, since it's currently the default in meta-raspberrypi. You can easily find the default version for meta-raspberrypi by searching for PREFERRED_VERSION_linux-raspberrypi, like this:

```
$ cd meta-raspberrypi
$ git grep PREFERRED_VERSION_linux-raspberrypi
conf/machine/include/rpi-default-versions.inc:PREFERRED_VERSION_linux-raspberrypi
?= "4.1.1%"
```

Start by downloading the kernel and then go to the 4.1 branch:

```
$ cd /path/to/workdir
$ git clone https://git.kernel.org/cgit/linux/kernel/git/stable/linux-stable.git/
$ git checkout -b linux-4.1.y -t remotes/origin/linux-4.1.y
```

Remember that when building for the Raspberry Pi, https://github.com/raspberrypi/linux.git is the supported kernel repository. Use it like this:

### Note

```
$ git clone https://github.com/raspberrypi/linux.git
$ git checkout -b rpi-4.1.y -t remotes/origin/rpi-4.1.y
```

Next, we can start working with our kernel. First, we need to decide what configuration to use; all existing configurations exist under arch/*/configs/ in the kernel tree. Later, we will look at how to make our own customizations to the kernel, but for now, we will start by using a predefined configuration for the ARM platform. When building from your host machine, you need to set up a cross compiler for the ARM architecture, which in this case is bcm2835 (which happens to be the CPU family of the Raspberry Pi). Use this command:

```
$ make O=/path/to/output_dir CROSS_COMPILE=/path/to/arm-linux-gnueabi- ARCH=arm bcm2835_defconfig
make[1]: Entering directory '/path/to/output_dir'
    HOSTCC  scripts/basic/fixdep
    GEN     ./Makefile
    HOSTCC  scripts/kconfig/conf.o
    SHIPPED scripts/kconfig/zconf.tab.c
    SHIPPED scripts/kconfig/zconf.lex.c
    SHIPPED scripts/kconfig/zconf.hash.c
    HOSTCC  scripts/kconfig/zconf.tab.o
    HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
```

```
make[1]: Leaving directory '/path/to/output_dir'
```

The command will ensure that the chosen configuration, a makefile, and a symlink to the original source tree are stored in the output directory. It's not mandatory to use an output directory; you can build directly within the kernel tree itself. However, it's strongly recommended to specify an output directory to avoid problems in your main kernel tree and keep it clean from build results. Next, we can start building the kernel. If building from a multi-core machine, it's recommended to use the -j flag in order to speed up the build.

```
$ make O=/path/to/output_dir CROSS_COMPILE=/path/to/arm-linux-gnueabi- ARCH=arm -j4
make[1]: Entering directory '/path/to/output_dir'
  GEN     ./Makefile
  scripts/kconfig/conf --silentoldconfig Kconfig
make[1]: Leaving directory '/path/to/output_dir'
make[1]: Entering directory '/path/to/output_dir'
  CHK     include/config/kernel.release
  GEN     ./Makefile
  WRAP    arch/arm/include/generated/asm/bitsperlong.h
  WRAP    arch/arm/include/generated/asm/cputime.h
  WRAP    arch/arm/include/generated/asm/current.h
  ...
  OBJCOPY arch/arm/boot/zImage
  Kernel: arch/arm/boot/zImage is ready
make[1]: Leaving directory '/path/to/output_dir'
```

If you want to change the configuration of your kernel, a predefined target called menuconfig can be used. It will give you a menu that contains entries for all the configurable parts of your kernel. It's possible to search for keywords in the menu to find a requested configuration option; this can be achieved by typing / followed by the keyword. Typing ? in the menu will display a short description of the configuration option you selected.

```
make O=/path/to/output_dir CROSS_COMPILE=/path/to/arm-linux-gnueabi- ARCH=arm menuconfig
```

Here is an example of using menuconfig. The M symbol means that the driver will be built as a separate module that can be loaded/unloaded by the kernel on demand.

![](img/76681dcfb8cd2bd107d0c06ab088f916_189_0.png)

When the new configuration has been saved, you can build the kernel in the same way as described earlier in the chapter.

## Configuring the kernel in Yocto

When using Yocto, it's possible to use BitBake for configuring and compiling the kernel. The following example will show you how to perform these steps in the Yocto Project, using the Raspberry Pi kernel as an example:

```
$ cd /path/to/project
$ bitbake linux-raspberrypi -c menuconfig
```

Perform your modifications and save the new defconfig, and then run this command:

```
$ bitbake linux-raspberrypi
```

As described in earlier chapters of this book, these commands will set up the cross compiler for you under the hood. In the Yocto Project, it's possible to take a customized kernel configuration file (.config) and put it into the kernel recipe. Then, you will have to name it defconfig and point it out in the SRC_URI variable for the recipe. This will cause the kernel to be built using the defconfig file as the default configuration. It's also possible to use a feature called fragments, which makes it possible to add a fragment of the kernel configuration options in a file called <somename>.cfg. When added to the SRC_URI variable, it will update the kernel configuration with the information from the fragment. For linux-raspberrypi (the kernel recipe in meta-raspberrypi), it's not possible to use defconfig or
