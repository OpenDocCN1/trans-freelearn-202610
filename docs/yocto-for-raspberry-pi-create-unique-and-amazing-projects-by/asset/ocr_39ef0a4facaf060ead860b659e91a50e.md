# # Theory

A layer, in the technical sense of the term, has some properties. They are as follows:

+   - **Name**: This usually starts with the string meta- (mandatory for better comprehension).
+   - **Priority**: This is the value used by BitBake to decide which recipe to use and the order in which the .bbappend files should be joined. It means that if two layers include the same recipe ( .bb) file, the one with the higher priority is used.

# ## Note

For example, a layer with a priority value set to 6 has a higher chance of its recipe being used than one with a priority value set to 5.

Furthermore, each layer is listed according to its functionality on this page:

http://layers.openembedded.org/layerindex/branch/master/layers/

| meta-parallelia | Support for the Parallelia board from Adapteva | Machine (BSP) | https://github.com/nathanrossi/meta-parallelia.git |
| --- | --- | --- | --- |
| meta-picosam9 | Picosam9 board support | Machine (BSP) | https://gitgoriuos.org/picopc-tools/meta-picosam9.git |
| meta-qemu-bsps | layer for other qemu machines | Machine (BSP) | https://github.com/akuster/meta-qemu-bsps |
| meta-raspberrypi | Raspberry Pi board support | Machine (BSP) | git://git.yoctoproject.org/meta-raspberrypi |
| meta-renesas-rza1 | Official BSP layer for Renesas RZ/A1 platforms | Machine (BSP) | git://github.com/renesas-rz/meta-renesas-rza1 |
| meta-rockchip | Rockchip ARM hardware support | Machine (BSP) | git://github.com/linux-rockchip/meta-rockchip |
| meta-romley | Intel Romley platform BSP | Machine (BSP) | git://git.yoctoproject.org/meta-intel |
| meta-samsung | Samsung smartphone support | Machine (BSP) | git://github.com/shr-distribution/meta-smartphone.git |
| meta-sugarbay | Intel Sugar Bay platform BSP | Machine (BSP) | git://git.yoctoproject.org/meta-intel |

+   - **The software layer**: This layer provides software families that can be used on any architecture (x86_64, ARM, PPC, MIPS, and so on). For example, we have meta-java (for Java support), meta-efl (for the Enlightenment environment), meta-qt5 (Qt support), meta-webserver (provides support for building web servers), meta-browser, and so on.
+   - **Miscellaneous**: If your layer doesn't fall into any other category, you can choose this type; however, there shouldn't be too many miscellaneous layers-it may be an indication that the purpose isn't well defined or that you should consider splitting the layer.
+   - **Machine (BSP)**: These are layers specific to a machine (such as meta-raspberrypi, meta-fsl-arm, and meta-ti). They contain machine configuration files and recipes to configure packages for the machines.
+   - **Distribution**: These are layers adding a distribution to the environment (such as meta-ivito add in-vehicle infotainment (IVI) support, meta-angstrom, and so on).

At this stage, we know how the layers are distributed within the Yocto Project. Now, we will see how