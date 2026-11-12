![](bbox=[0.0, 0.0, 1.0, 0.42])

When the new configuration has been saved, you can build the kernel in the same way as described earlier in the chapter.

# Configuring the kernel in Yocto

When using Yocto, it's possible to use BitBake for configuring and compiling the kernel. The following example will show you how to perform these steps in the Yocto Project, using the Raspberry Pi kernel as an example:

```
$ cd /path/to/project
$ bitbake linux-raspberrypi -c menuconfig
```

Perform your modifications and save the new defconfig, and then run this command:

```
$ bitbake linux-raspberrypi
```

As described in earlier chapters of this book, these commands will set up the cross compiler for you under the hood. In the Yocto Project, it's possible to take a customized kernel configuration file (.config) and put it into the kernel recipe. Then, you will have to name it defconfig and point it out in the SRC_URI variable for the recipe. This will cause the kernel to be built using the defconfig file as the default configuration. It's also possible to use a feature called fragments, which makes it possible to add a fragment of the kernel configuration options in a file called <somename>.cfg . When added to the SRC_URI variable, it will update the kernel configuration with the information from the fragment. For linux-raspberrypi (the kernel recipe in meta-raspberrypi), it's not possible to use defconfig or