
The main program is an endless loop of code or an internal-timed interrupt. Not all MCUs offer internal-timed interrupts, but what is critical is that it always runs at a selected time, for example, every 60 seconds. On an MCU, this is guaranteed to run every 60 seconds to the microsecond. This is crucial to make sure that you can run an algorithmic function, which uses your wind counter to save the wind speed. On an event driven OS, you may get highly inaccurate readings caused by delayed processor queues or the optimized execution.

## Analog inputs

The Raspberry Pi lacks analog inputs. MCUs usually have more analog inputs than digital ones. On tiny versions, they may even completely lack digital inputs.

Analog inputs will be very useful to read the direction of the wind, as wind vanes usually output a calculated sub-voltage from the input voltage to inform you of the direction. Something like a potentiometer, but more simply, it is like various resistors connected to a reed switch, which will activate a given circuit.

There are still a lot of other applications that can be useful for analog inputs.

## Parts required

The most affordable and pretty complete package is offered by Sparkfun—a wind vane, anemometer, and rain counter, with cables and a complete installation mount. Adding extras to this, such as instruments to measure UV light, atmospheric pressure, and temperature, is a breeze.

You will also need a Raspberry Pi and a programmed Arduino MCU or similar.

## Storing data on the Cloud

At some point of any project, you will, most certainly, want to store collected data somewhere. Usually, this is from collecting sensors, but it could also be debugging data.

The Cloud is a network of servers that can provision virtual machines for any use. Usually, you start from one server, but you always have the ability to increase the size or bundle several virtual machines to act as one endpoint. All this is usually handled by a nice frontend, such as Azure, Amazon AWS, or the Google Cloud platform.

## Phant

Phant is a logging tool developed by Sparkfun Electronics and is ready to be used free of charge at https://data.sparkfun.com; it stores data in JSON format. JSON is easy to read and compress, but most importantly, it's dynamic; dynamic meaning that you don't build relationships between each data packet, as in traditional SQL servers. This means that each data packet can contain any variety of structures that you save to Phant.

You can find yourself a very cheap Linux-based virtual machine and install your own Phant platform to take full control of your cloud data. This is an extremely easy way to allow you access to shared data anywhere in the world, as long as you have an Internet connection.

## Summary

In this chapter, you learned the basics of electrical engineering and the external system's integration. The possibilities are endless and only confined to your own imagination. In case you ever seem to get stuck, it is worth visiting some of these great sites for more information.

For Stack Exchange, use the following:

- Electrical Engineering: http://electronics.stackexchange.com/
- Raspberry Pi: http://raspberrypi.stackexchange.com/
- Stack Overflow (Programming): http://stackoverflow.com/
- Ask Ubuntu (Linux related): http://askubuntu.com/

For other sites, use the following:

- Arduino Forums: https://forum.arduino.cc/
- PICAXE Forums: http://www.picaxeforum.co.uk/
- Searching the internet

Thank you for your interest in this book, and I wish you happy prototyping!

## Index

## Symbols

## A

## B

## C

.NET
about 48
and ASP.MVC 48
DNX, installing 50
MVC 52
MVC Website, running 50, 51
nginx 52
other .NET applications 52
prerequisites 49
adapters
URL 17
admin configuration
about 128
apps, installing 129
Calendar Plus 130
other apps 130
server admin 128
AJAX GET Request
URL 48
Alamode 6
alsamixer
using 96
Apache
versus Kestrel 39
versus nginx 39
Aplay
for audio playback 95
Apple Filing Protocol (AFP)
about 67
Netatalk, configuring 67
Netatalk, installing 67
shares and time machine 68
Arduino
Forums, URL 144
used, for data accuracy 141
Ask Ubuntu (Linux related)
URL 144
ASP.MVC 48
audio, playing
about 95
AirPlayer, using 96
alsamixer, using 96
Aplay, for audio playback 95
OMXPlayer, for audio playback 95
audio recording 6
battery power source
about 101
capacity 107, 108
hardware requirements 101, 102
implementing 107
BitTorrent Sync
about 68
BTSync 2, installing 68-70
boot process 10
Broadcom chip 35
BTSync 2
installing 68-70
CAN bus 7
Cloud
data, storing on 143
Phant 144
Crystal Disk Mark 12

## D

daily functionality
about 131
World Wide Access 131
data
accuracy, Arduino used 141
database, installing
MySQL, installing 54
phpMyAdmin, installing 55, 56
SQLite, installing 56, 57
decoders
enabling 35
licenses, buying 35
distribution
updating 32
Dot Net Execution Environment (DNX)
about 49
installing 50
Dot Net Package Updater (DNU)
about 49
installing 50
Dot Net Version Manager (DNVM)
about 49
installing 50
drive, formatting
disk, remounting after reboot 63
drives, mounting 62
EXT4 partition, creating 62
drivers
listing 60
Dynamic DNS
about 28
client, installing 28, 29
free network 29
## E
Electrical Engineering
URL 144
eth0 port 16
Ext4
about 61
partition, creating 62
external storage
connecting 59
## F
FastCGI 53
files, accessing
about 63
FTP service 63
File Transfer Protocol (FTP) service
about 63
FileZilla, connecting with 64
WinSCP, connecting with 64, 65
FileZilla
connecting with 64
Fingerprint scanners 8
firmware
updating 32
Freeciv
about 76
configuring 76
installing 76
playing 77
## G
games servers 74
## H
hardware, limitations
about 11
network speeds 11
time 12
USB bottlenecks 11
hardware RAID
about 70
configuring 70
massive storage 71
redundant storage 72
hardware requirements, battery power source
battery packs 103
charging station 102
voltage regulator 104, 105
hardware watchdog
about 33
enabling 34
testing 35
HDMI Pi 94
HDMI to VGA converter 6
home automation 7
HTTP streaming
  about 84, 85
  object detection and tracking 88
  overlay watermark, removing 88
  text overlay 87
  web conferencing 85, 86
  X desktop, streaming 86
HUD sample
  about 88
  inline variables 89, 90
  overlay binary, using 89
  overlay code, compiling 90
## L
licenses
  URL 35
Local Area Network (LAN)
  about 15, 16
  Bcast 16
  eth0 port 16
  HWaddr 16
  inet6 addr 16
  inet addr 16
  loopback (LO) interface 17
  Mask 16
  wlan0 interface 17
## I
inline variables 90
Internet 3G dongles 5
internet configurations
  about 26
  business packages 27
  home packages 27
Internet of Things (IoT)
  about 110, 134
  first boot 111
  Python, for Linux 110
  Python, for Windows 7 and 8 110, 111
  remote connections 112
  requisites 110
  URL 110
iPerf 25
## M
Macintosh
  Apple Filing Protocol (AFP) for 67
media add-ons
  Alamode 6
  HDMI to VGA 6
  IR receivers 6
  Multiscard readers 6
  TV and radio receivers 6
  Webcams 6
Micro Controller Unit (MCUs) 142
Minecraft
  about 78
  configuring 79
  installing 79
  playing 80
MPEG-2 36
MVC
  and nginx 52
MySQL
  installing 54, 55
## J
Java Hard-Float
  installing 78
JavaScript web programming language 45
Jessie
  updating to 73
## N
Netatalk
  configuring 67
  installing 67
network
  bandwidth, recommended 25
  basic tests 23, 24
  benchmarking 23
  benchmarking tools 24
## K
Kestrel
  versus Apache 39
  versus nginx 39
iPerf 25
speedtest application 24
testing 23
Network Attached Storage (NAS) 4, 62
nginx
extra frameworks 43
installing 40, 41
versus Apache 39
versus Kestrel 39
virtual hosts, configuring 41, 42
Node.js
about 45
installing 45
Node Package Manager (NPM)
about 46
Node.js server 46, 47
URL 46
NO-IP domain
URL 29
NOOBS
URL 9
## O
Ohm's law
about 134
power (W) 135-137
OMXPlayer
for audio playback 95
for video playback 95
OpenArena
about 77
configuring 77
installing 77
playing 78
OpenTTD
about 74
configuring 75
installing 75
lan_internet 75
playing 75
server_advertise 75
server_bind_ip 75
server_name 75
server_port 75
OSMC
about 96
add-ons, using 98
AirPlay 98
CEC, enabling 99
configuring 97
installing 96
media sources 98
other codecs, enabling 97
overclocking 99
performance optimization 99
remotes, enabling 99
wireless configuration 97
overlay binary
URL 88
using 89
overlay watermark
removing 88
ownCloud
about 121
admin configuration 128
first configuration 126, 127
MySQL 122
nginx and PHP 122-125
permissions 126
requisites 121
URL 129, 130
## P
packages
updating 32
peripherals
about 4, 120
Bluetooth 120
CAN bus 7
fingerprint scanners 8
home automation 7
Internet 3G dongles 5
Joysticks 7
keyboards and mice 5
URL 5, 120
USB hubs 5
USB missile launcher 7
USB to SATA 7
weather station 8
wireless 120
Wireless USB network adapters 5
Phant 144
