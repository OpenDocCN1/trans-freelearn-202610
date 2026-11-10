
The parts of the official Raspberry Pi case can be individually unclipped, offering fast access to the GPIO pins on the board inside

The case was designed by Kinneir Dufort (magpi.cc/2brbXLu). It's an award-winning design team that has done a great job

The official case provides easy access to all of the ports on the Raspberry Pi, and the micro SD card can be removed without dismantling the case

## HDMI cable
An HDMI cable is the easiest way to connect your Raspberry Pi to a computer monitor or television. You don't need an expensive one, and most people recycle one from an old games console or DVD player.

## USB power
A good 2A or 2.5A power supply provides you with enough power to run a Raspberry Pi with all kinds of peripherals connected. You can buy an official Universal Power Supply (magpi.cc/2a14pye).

## Keyboard
Any standard USB keyboard can be used to enter commands to your Raspberry Pi. You can use a Bluetooth keyboard with the Raspberry Pi 3, or any other Pi with a Bluetooth dongle attached. A wired keyboard is easier to use when setting up your Raspberry Pi.

## Mouse
Any standard mouse will work with the Raspberry Pi, although ones with two buttons (non-Apple mice) work better. Like keyboards, a Bluetooth mouse will work once it's paired, but a wired mouse works as soon as you plug it in.

![](img/d8322379620245fcd21eccc3855b8c78_10_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_10_1.png)

![](img/d8322379620245fcd21eccc3855b8c78_10_2.png)

![](img/d8322379620245fcd21eccc3855b8c78_10_3.png)

![](img/d8322379620245fcd21eccc3855b8c78_10_4.png)

## INSTALLING RASPBPIAN
### Discover how to use NOOBS to quickly set up the Raspbian operating system on your Raspberry Pi
Before you start using your Raspberry Pi, it needs to have an operating system (OS). This is the software used to start the hardware, and open and close programs.

Many computers use a specific operating system tied to the hardware. You’ll probably be used to Windows on a PC and OS X on a Mac computer.

Most Raspberry Pi owners use an open-source operating system called Raspbian, which is based on Linux. The current version is based on a version of Linux called Debian Jessie, hence the name Raspbian (sometimes you’ll hear it called ‘Raspbian Jessie’).

Linux is like Windows and Mac OS X, but more fun because it’s open-source, so anybody can view the source code and improve it.

You can install a range of different OSes on a Raspberry Pi, some based on other versions of Linux, others based on Windows, and even completely unique environments like RISC OS.

Raspbian is the official OS and the one most beginners should start with. It’s the simplest to install, easiest to use, and most projects and tutorials use Raspbian as their base.

### Start with NOOBS
There are two approaches to installing Raspbian and other operating systems. Beginners should start with NOOBS (New Out Of Box Software). More advanced users may copy an image file containing a whole operating system directly to the SD card.

First, you must format your micro SD card to use the Windows FAT 32 format. The easiest way to do this on a Mac or Windows PC is to use a program called SD Card Formatter (magpi.cc/2bncvkm).

Connect your micro SD card to a Mac or Windows PC, typically using a micro SD-to-SD card adapter or a USB card reader, and use SD Card Formatter to erase the card.

Next, download the NOOBS ZIP file from magpi.cc/2bnf5XF. Extract the contents of the file and open the NOOBS folder. Copy the contents across to the root of the SD card. See the ‘Setting up NOOBS’ steps for more information.

## AVAILABLE OSES
### Raspbian
The official operating system is the easiest to use, and the one beginners should start with. It works a lot like other popular operating systems.

![](img/d8322379620245fcd21eccc3855b8c78_11_0.png)

### Windows 10 IoT Core
Not the full version of Windows, sadly, but Windows 10 IoT Core enables programmers to run Internet of Things and embedded projects.

![](img/d8322379620245fcd21eccc3855b8c78_11_1.png)

### Ubuntu MATE
Ubuntu is one of the world’s most popular Linux operating systems, and Ubuntu MATE is a lightweight version that runs just fine on the Raspberry Pi.

![](img/d8322379620245fcd21eccc3855b8c78_11_2.png)

NOOBS automates the process of installing Raspbian. Select the Raspbian option and click on Install to run it

![](img/d8322379620245fcd21eccc3855b8c78_12_0.png)

With the NOOBS files copied across, remove the micro SD card from your computer and slot it into your Raspberry Pi. Now connect the keyboard, mouse, and HDMI cable. Finally, attach the USB power to boot up the Raspberry Pi.

The Raspberry Pi will boot, displaying the NOOBS installer. By default it has one option, ‘Raspbian [RECOMMENDED]’. Place a tick next to Raspbian and click Install. Click Yes in the Confirm alert to begin installing Raspbian.

Now you just need to wait while the Raspbian file system is extracted. When it’s finished, you’ll see the Raspbian desktop and the message ‘OS(es) Installed Successfully’. Click OK to start using your Raspberry Pi.

### Installing image files
Installing an operating system from an image file is a slightly more complex procedure, but one that more advanced (and Pi Zero) users should learn. Image files are copied differently in Windows, compared to Linux and Mac computers.

In both systems, you format the micro SD card to FAT 32 as usual,

NOOBS automatically copies all the files needed to run Raspbian onto your SD card

![](img/d8322379620245fcd21eccc3855b8c78_12_1.png)

then you download the operating system as an image file, a large file ending in ‘.img’. This file is then copied bit by bit as an exact replica to the micro SD card.

On a Windows PC, you will copy the image file using an app called Win32DiskImager (magpi.cc/2bndEsr). On Mac and Linux machines, most users copy the file using a command called ‘dd’ in the terminal.

Full instructions for copying image files for Windows, Mac, and Linux can be found on the Raspberry Pi website (magpi.cc/1V5Oj8E).

A good alternative for Mac owners is a program called Apple Pi Baker (magpi.cc/2bcD53z). This program enables you to pick the image file and the SD card, and then handles the copying automatically.

Learning how to copy image files is essential if you want to use operating systems other than Raspbian. Beginners should stick with NOOBS to install Raspbian to start with. It’s much easier and is the best operating system for beginners.

## SETTING UP NOOBS
### Download NOOBS
In a browser, visit magpi.cc/2bnf5XF. Click Download ZIP to get all the files. Open your downloads folder and locate the NOOBS file: currently it’s ‘NOOBS_v1_9_2’. Right-click on a Windows PC and choose Extract All, then Extract. Just double-click the file on a Mac to extract it.

![](img/d8322379620245fcd21eccc3855b8c78_12_2.png)

### Format SD card
Open SD Card Formatter and you’ll see the card in the Drive letter. Change the Volume Label to BOOT so you can identify it later. Now click Option and change Format Type to Full (Erase). Ensure Format Size Adjustment is set to Off and click OK. Click Format, then OK. Click Exit to close SD Card Formatter when it’s finished.

![](img/d8322379620245fcd21eccc3855b8c78_12_3.png)

### Copy NOOBS files
Open the freshly extracted folder so you can view all the files. It should have folders called defaults, os, and overlays, and files including bootcode.bin and recovery. Select all of the files and drag them onto the BOOT icon in the sidebar. This copies all of the files inside the NOOBS folder to the root of the SD card. It’s important to copy the files inside NOOBS, and not the NOOBS folder itself.

![](img/d8322379620245fcd21eccc3855b8c78_12_4.png)

### OSMC
OSMC (Open Source Media Centre) is an easy way to transform your Raspberry Pi into a video and audio player.

![](img/d8322379620245fcd21eccc3855b8c78_12_5.png)

### RISC OS
RISC OS is an operating system originally designed by Acorn Computers for ARM-based systems. It’s very light and completely different.

![](img/d8322379620245fcd21eccc3855b8c78_12_6.png)
