# # Downloading the Raspbian image

You'll need to grab the latest **Raspbian OS** image from the Raspberry Pi site at `https://www.raspberrypi.org/downloads`.

Download the Raspbian OS ZIP file containing the image to your PC.

> At the time of writing, the latest version was Raspbian Jessie version 4.1 (`2015-09-24-raspbian-jessie.zip`).

Once downloaded, unzip the file and you'll have the file, `2015-09-24-raspbian-jessie.img`.

The next thing to do is burn this image to your SD card…

# ## Using Microsoft Windows

On a Windows PC, the best way to burn the image to your SD card is to use the **Win32 Disk Imager** utility. This can be downloaded from `http://sourceforge.net/projects/win32diskimager`.

> The current version, at the time of writing, is 0.9.5.

It doesn't have an installer, and launches directly from the EXE file.

Now, it's time to create your SD card image:

+   1. Insert your SD card into the PC and launch the Win32 Disk Imager.
+   2. Select the SD card device drive letter (make sure it's right!).
+   3. Choose the Raspbian image file you've just downloaded.