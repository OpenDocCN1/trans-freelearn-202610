# # Texture performance enhancements

Let's explore some changes we can make to our texture files, which might help improve performance, depending on the situation and the content of the files we're importing. In each case, we'll explore the changes that need to be made and the overall effect they have, whether this results in a positive or negative impact on memory or CPU, an increase or decrease in the texture quality, and under what conditions we can expect to make use of these techniques.

# # Reduce texture file size

The larger a given texture file, the more GPU Memory Bandwidth will be consumed, pushing the texture when it is needed. If the total memory pushed per second exceeds the graphics card's total Memory Bandwidth, then we will have a bottleneck, as the GPU must wait for all textures to be uploaded before the next rendering pass can begin. Smaller textures are naturally easier to push through the pipeline than larger textures, so we will need to find a good middle ground between high quality and performance.

The exact compression format Unity chooses can be overridden, although the available options vary per platform, since practically every platform has its own custom formats that work best for it. If we click on one of the platform-specific tabs beside the **Default** tab (just above the **Max Size** option), we will expose settings for a specific platform and can choose the exact compression format we want Unity to use.

There is also the **Crunch Compression** setting, which will apply an additional level of lossy compression on top of the DXT compression format. This option is only revealed if the other compression settings resulted in a DXT level of compression. This setting can save even more space at the cost of potentially glaring compression artifacts, depending on the **Compressor Quality** setting.

Several of a texture's import settings are fairly mundane, such as determining whether the file contains an alpha channel, how to wrap the texture at its extents, filtering method, and maximum possible resolution of the file (a global limit so that we don't accidentally overscale the texture beyond its original size on certain platforms). However, there are several other interesting options in these import settings, which we will cover in other sections where appropriate.

![](bbox=[0.137, 0.243, 0.203, 0.302])