information in the BitBake file, such as LIC_FILES_CHKSUM, will be based upon this particular SRCREV value. The complete con2fbmap_git.bb file looks like this:

```
```
SUMMARY = "Utility for swapping an fbft-based device"
DESCRIPTION = "Simple utility for swapping an fbft-based device as \
the console"
LICENSE = "GPLv3"
LIC_FILES_CHKSUM = "file://COPYING;md5=d32239bcb673463ab874e80d47fae504"
SECTION = "console/utils"
Inherit autotools
SRC_URI = "git://gitlab.com/pibox/con2fbmap.git;branch=master;protocol=https"
SRCREV = "f57bf6d28910ba665efab8a65085ba6d4c6162a5"
S = "${WORKDIR}/git"
```
```

Remember to build and verify the new recipe. When you are satisfied with your changes, you can go ahead and create your new Git commit. Make sure that you follow the guidelines when writing your commit message.

```
```
$ git add con2fbmap_git.bb
$ git commit -s
con2fbmap: Add new recipe
Currently there is no easy way to manually switch between
framebuffer devices and console during run-time.
This tool can be used when the system is up and running, to
set and show mapping between framebuffer tft devices
(such as pitft displays) and the consoles.
Signed-off-by: Petter Mattson <petter@technux.se>
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```
```

Once you are satisfied with your commit message, it's time to generate the patches. Since all changes in this example will fit into one Git commit, we can skip the --cover-letter flag.

```
```
$ git format-patch --subject-prefix='meta-raspberrypi][PATCH' origin
0001-con2fbmap-Add-new-recipe.patch
```
```

When the tool has been built and carefully tested on the target, we can send it to the mailing list:

```
```
git send-email --to yocto@yoctoproject.org 0001-con2fbmap-Add-new-recipe.patch
```
```

You have now learned how to contribute to the main layer. A good piece of advice is to be patient when working with an open source community. Depending on which state the project is in, maintainers and other people responsible for integrating changes might be busy, and sometimes, it takes time until they respond to your patch. If you haven't heard anything in a while, don't be afraid to send a friendly reminder to people on the list to look at your patch.