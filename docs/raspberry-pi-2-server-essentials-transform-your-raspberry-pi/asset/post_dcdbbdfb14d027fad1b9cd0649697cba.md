
## Fun peripherals

You might have some of these lying around in your gadget box. Hopefully, reading about some of these less used devices might spark some creative ideas:

- **Joysticks**: Microsoft's Xbox 360 controller works like a mouse in X using **xboxdrv**, which can be installed from the package repository. Other joysticks might need a ported driver that can be found on Internet forums.
- **USB to SATA**: You can purchase simple USB to SATA controllers that allow you to attach SATA hard drives using dedicated power supplies. The real fun begins when you use hardware RAID-based USB to SATA controllers that can be chained in various configurations, which can give you massive storage, high redundancy, or maximum performance.
- **CAN bus**: The CAN bus is the standard used in all modern motor vehicles. It is a standard port that gives mandatory data that can be interpreted by anybody, for example, throttle value, misfiring of cylinders, or air to fuel ratio. PEAK-System has a variety of peripherals and software that are compatible with the Raspberry Pi. If you have access to manufacturer-specific codes, you can even adjust engine mappings with these tools.
- **Home automation**: A compatible device called TellStick runs well as a third-party, home automation device for the Raspberry Pi, but as an advanced Linux user, you should strive to make your own applications using real-time microcontrollers, such as Arduino on AlaMode, or the very cheap PICAXE microcontrollers.
- **USB missile launcher**: Available on a variety of websites and stores, this is the perfect gadget if you need to shoot plushy missiles at unidentified objects! This is purely an entertainment peripheral, but you could use it for DIY projects as well.

> Be careful, though, as the maximum throughput speed you can achieve is governed by the bandwidth of USB 2.0. In theory, this is a maximum speed of 60 MBps, but is shared by all the devices on the controller and not per port. There is more information about this later in the book.

> Adjusting non-standard values may damage your ECU and will void any warranties.

- **Fingerprint scanners:** Futronic's fingerprint scanners work well with Raspbian, and there are many examples that can be found online. They are standalone programmable devices that communicate with the Pi using simple messages over USB-UART and have extensive documentation available with the device.
- **Weather station:** Sparkfun sell a pretty complete weather station that includes a wind vane, anemometer, and tipping bucket rain gauge. The kit includes clamps and mounting masts. You can add a light, temperature, and humidity sensor easily and without breaking the bank. The challenge depends on using a real-time microcontroller and building your own database to log all the data. But don't worry, there are many tutorials online that will help you progress further.

## Installing Raspbian on the Raspberry Pi

There are many distributions that can run on the Pi. Some are specific real-time operating systems, such as RISC OS, or mainstream operating systems, such as Raspbian or Archlinux. A few flavors of home media centers, such as OpenElec or OSMC (previously known as XBMC). As of June 2015, you will also be able to install Windows 10 Internet of Things. No Android support is foreseen for the near future.

The Raspberry Pi Foundation recommends that you use their image called NOOBS (New Out Of Box Software), which contains frequently updated list of different operating systems. In this book, we will be using Raspbian. It is supported by the Foundation and has the best compatibility with ease of use. Raspbian is based on Debian and is similar to many other Linux operating systems. The steps for installing Raspbian are as follows:

1. For Windows and Macintosh users, it is recommended by the Raspberry Pi Foundation that you use the SD Formatter from `http://www.sdcard.org/`.
   For Windows, perform the following steps:
   1. Install the SD card formatting tool.
   2. Set the **Format size adjustment** option to **ON** in the **Option** menu.
   3. Make sure you've selected the correct SD card.
   4. Click on the **Format** button.
   For Macintosh, perform the following steps:
   1. Install the SD card formatting tool.
   2. Select **Overwrite Format**.
   3. Make sure you've selected the correct SD card.
   4. Click on the **Format** button.

For Linux, perform the following steps:
   1. It is recommended that you use the GParted or Parted tool in Linux.
   2. Format the entire disk as FAT.

2. You should download the latest NOOBS archive from http://www.raspberrypi.org/downloads/
3. Unzip the archive.
4. Copy the extracted files onto the formatted SD card.
5. Insert the SD card into the Raspberry Pi. Plug in your HDMI or other video cable with a compatible keyboard and power it up.
6. The Pi will boot up and present the list of operating systems; select **Raspbian**.
7. If your display is blank, try to press the numeric keys, as listed here, while the Pi is booted up:
   - 1: HDMI mode
   - 2: HDMI safe mode
   - 3: Composite PAL
   - 4: Composite NTSC

## Understanding the design of the Raspberry Pi

The Raspberry Pi has two identifiable microchips on the PCB:

- In the center is one that's clearly marked with a Broadcom logo and text starting with BCM283x, which is the main processor
- The Raspberry Pi 2 has its RAM chip at the back of the PCB.
- Near the USB port, there is a smaller chip that is either a USB hub or a USB/LAN chip, depending on the model.

BCM283x is actually a high-performance GPU with an embedded ARM processor. It is a SoC (System on Chip), which means that there is small amount of space for code that executes when it gets turned on. This is known as Stage 1 in the boot process.

## Boot process

Some network actions need to be performed during the boot process, and it is good to understand the various stages in case you need to troubleshoot something. The boot process is as follows:

1. Stage 1 begins on the GPU and executes the code SoC firmware, which starts to load Stage 2 code to the L2 cache.
2. Stage 2 reads `bootcode.bin` from the SD card. It initializes SDRAM (synchronous dynamic random access memory) and loads Stage 3.
3. Stage 3 is the `loader.bin` file. This loads `start.elf`, which starts the GPU.
4. During `start.elf`, it prepares to load `kernel.img`.
5. The kernel image then reads `config.txt`, `cmdline.txt`, and `bcm283x.dtb`.
6. If the `.dtb` file exists, it is loaded at 0x100, and the kernel is loaded at 0x8000 in memory.
7. The kernel image is the first binary that runs on the ARM CPU, and it can be compiled with custom support for specific hardware.
8. The operating system starts to load.

All the source code in stages 1 to 3 are closed source and protected by Broadcom. These closed source files are compiled and released by Broadcom only; you can update them on your SD card by running a firmware upgrade in Raspbian, which is covered later.

The `kernel.img` file connects the application to the hardware. Any computer with an operating system has a kernel of some sort. In Linux, it is possible to compile your own kernel, and it might be the first file that you might want to amend yourself. This allows you to change the boot screen, load custom drivers, or perform other tasks that you might need. This is an advanced task and is not covered in this book.

## Other capabilities

BCM283x also has dedicated audio hardware together with video encoding/decoding. This allows the Raspberry Pi to playback HD (MPEG-4) content, such as videos, or render games using OpenGL ES. You can buy additional encoder/decoder licenses for extra functionality, such as MPEG-2, used in DVD video encoding and VC-1, which is used by Microsoft's WMV formats. This is also used for Silverlight live streaming.

The SD card is also directly interfaced by the Broadcom chip using dedicated hardware inputs/outputs and interrupts.
