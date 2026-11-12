
### Follow-up

By now, your patches should have reached the mailing list, and you can sit back for a while and wait for feedback. If you receive feedback from the mailing list, you are expected to reply with an answer saying that you will perform the requested changes or, if you disagree, reply with an 'appropriate reason for why you don't.

Don't be scared if you receive feedback; this is normal. In fact, many of the changes sent to the mailing list will need updates before they are ready to be integrated into the main layer.

When sending an update of your patch to the mailing list, it's important that you specify the version number of your patch series. This can easily be done by slightly modifying the line used when you generated the patch in the first place (pay attention to the new subject prefix):

```
$ git format-patch --cover-letter --subject-prefix='meta-raspberrypi][PATCH v2' origin
```

### Note

Remove "--cover-letter" if you only have one patch or don't feel the need for it.

If your patch needs yet another update, just increment the version number and try again. When your patch has reached the meta-raspberrypi layer, you are done and can continue working with some other feature that interests you.

### Practical example - sending a custom tool upstream

In this section, we will go through a practical example of integrating a custom tool into meta-raspberrypi. The tool we have chosen is **con2fbmap**, a command-line tool for setting and showing mappings between framebuffer TFT devices (such as PiTFT displays) and consoles.

First of all, we need to identify where in the layer structure we should place our new recipe. In some situations, we can guess the location by reading the name of the package. One example of this is the omxplayer recipe. It's quite easy to figure out that it should be placed among the multimedia recipes (recipes-multimedia). If we cannot use the package name to find a location, we can check whether a similar tool exists. For our example, meta-oe (the openembedded base layer) contains a somewhat similar tool called **fbset**. It's a tool for modifying framebuffer devices. If we locate the **fbset** recipe, we can see that it's placed within recipes-support, and we should probably place our package in that directory as well.

You can find the fbset tool at https://github.com/openembedded/meta-openembedded/tree/master/meta-oe/recipes-support/fbset, as shown here:

![](img/76681dcfb8cd2bd107d0c06ab088f916_221_0.png)

When creating a new recipe, you can either use the tools described in Chapter 7, Deploying a Custom Layer on the Raspberry Pi or create it from scratch. In this example, we will create it from scratch. The first thing we need to know is which kind of source (such as .tar.gz or Git) the tool has. In this case, the original source is Git, and we can create a basic recipe for con2fbmap, as follows:

```
$ cd meta-raspberrypi
$ mkdir -p recipes-support/con2fbmap
$ cd recipes-support/con2fbmap
$ touch con2fbmap_git.bb
```

Use your favorite editor and create the content for the BitBake file (.bb). In this book, we have chosen to use a "known state" and set SRCREV to **f57bf6d28910ba665efab8a65085ba6d4c6162a5**. Other information in the BitBake file, such as LIC_FILES_CHKSUM, will be based upon this particular SRCREV value. The complete con2fbmap_git.bb file looks like this:

```
SUMMARY = "Utility for swapping an fbft-based device"
DESCRIPTION = "Simple utility for swapping an fbft-based device as \
the console"
LICENSE = "GPLv3"
LIC_FILES_CHKSUM = "file://COPYING;md5=d32239bcb673463ab874e80d47fae504"
SECTION = "console/utils"
inherit autotools
SRC_URI = "git://gitlab.com/pibox/con2fbmap.git;branch=master;protocol=https"
SRCREV = "f57bf6d28910ba665efab8a65085ba6d4c6162a5"
S = "${WORKDIR}/git"
```

Remember to build and verify the new recipe. When you are satisfied with your changes, you can go ahead and create your new Git commit. Make sure that you follow the guidelines when writing your commit message.

```
$ git add con2fbmap_git.bb
$ git commit -s
con2fbmap: Add new recipe
Currently there is no easy way to manually switch between
framebuffer devices and console during run-time.
This tool can be used when the system is up and running, to
set and show mapping between framebuffer tft devices
(such as PiTFT displays) and the consoles.
Signed-off-by: Petter Mattson <petter@technux.se>
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```

Once you are satisfied with your commit message, it's time to generate the patches. Since all changes in this example will fit into one Git commit, we can skip the --cover-letter flag.

```
$ git format-patch --subject-prefix='meta-raspberrypi][PATCH' origin
0001-con2fbmap-Add-new-recipe.patch
```

When the tool has been built and carefully tested on the target, we can send it to the mailing list:

```
git send-email --to yocto@yoctoproject.org 0001-con2fbmap-Add-new-recipe.patch
```

You have now learned how to contribute to the main layer. A good piece of advice is to be patient when working with an open source community. Depending on which state the project is in, maintainers and other people responsible for integrating changes might be busy, and sometimes, it takes time until they respond to your patch. If you haven't heard anything in a while, don't be afraid to send a friendly reminder to people on the list to look at your patch.

### Summary

In this chapter, we had an overview of open source software and how to work with open source projects. Some of the major benefits of contributing to open source, such as decreasing integration cost for your product, were highlighted. We also learned the basics of Git and some more advanced Git commands, which can be used in combination with the sendmail binary, in order to send patches to an open source community. Finally, we took a walkthrough of a real-world example, by learning how to integrate the custom tool con2fbmap into meta-raspberrypi and send it for review to the Yocto Project community.

In the next chapter, we will summarize all that we have learned through this book by creating a home automation project.

## Chapter 12. Home Automation Project - Booting a Custom Image

The Internet of Things is a very hot subject today. Using automation within the home to ease daily tasks has gained more importance than ever. The Raspberry Pi, due to its size, price, and the possibilities to easily connect it to other electronics equipment, is a very good choice for making small home automation projects. This chapter will guide you through a small home automation project that will let you set up a remote lighting control system using the Raspberry Pi and then control it remotely with the help of a smartphone. The project will require skills learned in earlier chapters, so this chapter will also act as a summary of all you have learned throughout the book.

The chapter will start with an introduction to our home automation project and the material that is required for it. Then, it will go into the integration of Yocto with our project. This section of the chapter will explain how to set up the image used by the Raspberry Pi: it will cover things such as creating a new layer containing our new image recipe and adding/modifying packages required for our project. The chapter will end with a section that puts everything together. It will explain how all the physical parts should be connected and how to remote control the device using different devices, such as a smartphone or another Raspberry Pi with touchscreen.

### Home automation using a Raspberry Pi

Our home automation project will loosely continue from the “light an LED” example in Chapter 5, Creating, Developing, and Deploying on the Raspberry Pi. It will adapt and evolve that project to create one where the end user can wirelessly light a lamp using a smartphone (or some other device with a web browser).

As illustrated in the following diagram, the idea is to connect a one-channel relay module to the Raspberry Pi, using the GPIO pins. Then, we connect a lamp and an external power source to the relay module, creating a circuit. When everything has been connected, it will be possible to control the relay module (on/off) using GPIO connections from the Raspberry Pi.

![](img/76681dcfb8cd2bd107d0c06ab088f916_225_0.png)

A home automation project that enables remote lighting
