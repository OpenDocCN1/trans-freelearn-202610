
## Booting your Pi

You're now ready to boot up your Raspberry Pi. Pop your shiny new SD card into it and plug in the power.

Assuming that you have a monitor attached to your Pi, you should see your system booting up nicely. Although you could wait for it to boot up and connect to it via a terminal session (we'll look at that later), I recommend that you connect a monitor to it, at least in the first instance, just to make sure everything is working correctly.

In the new **Jessie** version of Raspbian, you'll boot straight into a desktop GUI, which is a major change from previous versions, where you'd be taken to the **raspi-config** utility, the first time the system is run, where you'd set up your Pi, and importantly, expand the file system to use the entire space available on your SD card.

![](img/631e8b200cce187082ee46b0eb1681b3_297_0.png)

*Debian Jessie boots into the GUI by default*

## Expanding the file system

When you first create your Raspbian SD card, you'll only be left with about 200Mb of space in the file system, regardless of the size of your SD card. This is not much use, so we want to expand the file system so that it uses all of the available space on the card.

Fortunately, this is very easy on the Raspberry Pi now, as this function is available in the Raspberry Pi Configuration Tool on the desktop.

To access the new configuration tool, go to **Menu** and select **Preferences | Raspberry Pi Configuration.**

![](img/631e8b200cce187082ee46b0eb1681b3_298_0.png)

The new Raspberry Pi Configuration Tool

### Goodbye GUI

> Most of our work is going to be done in the command-line interface (CLI). Therefore, before we reboot the system in a minute, let's change the **Boot** option by selecting **To CLI**, as shown in the previous screenshot, so boot into the command line going forward.

Anyway, now we click on the **Expand Filesystem** button, and in a couple of seconds, you'll see a confirmation message. The filesystem will be expanded when the system next reboots.

## Using the raspi-config utility

If you have an older version of Raspbian, or you're not using the desktop GUI, then you'll need to use the raspi-config utility (which is still better than the old days when we had to do this manually in the shell). The first time you boot up, you'll be taken straight to the raspi-config utility.

![](img/631e8b200cce187082ee46b0eb1681b3_299_0.png)

The first option is the Expand Filesystem option; select this and you'll see various commands scrolling up the screen. Once it's finished, you'll see the following message:

Root partition has been resized.

The filesystem will be enlarged upon the next reboot

Click on OK.

Select Finish on the config screen and reboot your Pi when prompted.

After your Pi reboots with its fuller file system, you'll be taken straight to the shell prompt where you can log in with the default user and password.

Login: pi

Password: raspberry

## Setting up your Pi

When you boot into the shell and have the Ethernet connected, hopefully the Pi will have connected to your home network and acquired an IP address from your router. If this is the case, you should see the **IP address** that has been issued just before the login prompt, as shown in the following screenshot:

![](img/631e8b200cce187082ee46b0eb1681b3_300_0.png)

As you can see from my screenshot, it's given me the IP address, `192.168.0.118`. This is good because I can now access the Pi remotely, using a secure shell (SSH) client to connect to it from the comfort of my laptop. This is particularly useful when my Pi is in the office and I want to sit on my sofa in front of the telly but still work on it, which I often do when I'm feeling lazy.

To do this, download PuTTY: a utility that allows you to connect to shell terminals remotely over the network. You can download it from http://www.putty.org.

Install and launch PuTTY and you're ready to connect to your Pi remotely from the comfort of your sofa.

![](img/631e8b200cce187082ee46b0eb1681b3_301_0.png)

## Setting Up Your Raspberry Pi

Type the IP address of the Raspberry Pi into the Host Name box and click on Open. You'll be connected to your Pi in a remote terminal window. Once you've logged in, you can do pretty much everything on your Pi, as if you were sitting in front of it.

![](img/631e8b200cce187082ee46b0eb1681b3_302_0.png)

We'll assume from now on that most of the work we do will be through a remote shell session, unless highlighted otherwise.

If you want to use the command line to launch the Raspberry Pi remote shell—for example, from another Linux system—use the following command from your terminal window:

```
# ssh pi@192.168.0.125
```

You'll then be prompted for the Pi's password and taken into a shell session.

![](img/631e8b200cce187082ee46b0eb1681b3_303_0.png)

The Pi shell session launched from a Debian desktop terminal window

## Getting up to date

Something that you should get into the habit of doing is updating the operating system regularly; even though you may have the latest image installed, it's very likely that there are updated packages available. To update your OS, enter the following command:

```
$ sudo apt-get update
```

After this, enter the following one:

```
$ sudo apt-get dist-upgrade
```

This may take a while, depending on the number of updates required.

## Getting the right time

The Raspberry Pi doesn't have on-board real-time clock hardware. This is one of the deliberate omissions to keep the cost of the board down. Instead, the Pi gets its time when it boots up from time servers on the Internet using the Network Time Protocol (NTP). However, if there is no Internet connection at the time of booting up, then the time will be wrong.

> In our security system, it's important that the time is kept accurate so that timestamps on log files and images are correct.

## fake-hwclock

The fake-hwclock package is included in the latest Raspbian distributions, but in other past versions it wasn't. If you need to install it, use the command that follows:

```bash
$ sudo apt-get install fake-hwclock
```

fake-hwclock is used by the Raspberry Pi to try and keep time when there is no network connection. It will regularly save the current time and restore it at boot-up. The obvious problem with this is that if the Pi has been switched off a few days, then the time will be set to the last time that it was on, using fake-hwclock.

If you want to see what time it last logged, type the following command:

```bash
$ cat /etc/fake-hwclock.data
```

## ntp

The **Network Time Protocol (NTP)** is used when there is an Internet connection available and it can request the latest most accurate time from one or more time servers on the Internet.

By default, the **ntp service** is enabled on the latest Raspbian distribution, but it will initially get its time at boot-up from fake-hwclock if there is no Internet connection. There may be times when it's necessary to force the ntp service to update from the Internet—for example, if the Internet connection is restored sometime after boot-up.

To force the ntp service to update from the Internet, use the following commands:

```bash
$ service ntp stop
$ ntpd -gq
$ service ntp start
```

## Talking of security…

There's no point in having a security system if the system itself is not secure. So, now we'll change the default password for the pi user.

From the prompt, type the following command:

```text
$ sudo passwd pi
pi@raspberrypi ~ $ sudo passwd pi
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

## What is this sudo thing anyway?

You'd have noticed that we've been putting `sudo` at the start of each command that we run in the terminal window. This is so that commands are run as the **root user** — the highest security level. This elevated security is required to perform many operations. `sudo` actually means *super do.*

If you can't be bothered to type `sudo` every time, then you can switch to the super user by typing the following:

```
$ sudo su
```

You'll see that the prompt changed from a $ to a #, which indicates that you are now running as the root user.

So, this might be a good time to change the root user password too! To do this, type the following:

```
# passwd
root@raspberrypi:/home/pi# passwd
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
root@raspberrypi:/home/pi#
```

## Connecting via Wi-Fi

You can also connect your Raspberry Pi to your network using Wi-Fi by plugging a USB dongle into it. There are additional configuration steps required to make this work, which are beyond the scope of this chapter, but there are many resources available covering this subject.

> You can find recipes for connecting your Raspberry Pi using Wi-Fi in the **Raspberry Pi Networking Cookbook** by Rick Golden, published by Packt Publishing (https://www.packtpub.com/hardware-and-creative/raspberry-pi-networking-cookbook).
