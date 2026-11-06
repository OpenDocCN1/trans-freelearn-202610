# SD cards

SD cards all look alike, but, in fact, some use a multitude of different controllers and NAND Flash memory chips. All these combinations make up the reliability and speed of the card; the cheapest is the worst, usually. That said, spending a fortune on an SD card is not a good idea either. The speeds of SD cards are marked with a numeric symbol ranging from 1, the slowest, to 10, the fastest. A class 6 or 8 SD card is a good balance between performance and price.

However, not all Class 10 cards are the best choice either. The only way to be sure is to use a trusted speed test application to verify that the card is performing at the advertised read and write speeds; this is the best way to identify counterfeit cards:

![](bbox=[0.31, 0.34, 0.69, 0.5])

The SD card should only act as the primary partition for an operating system and applications installed on it. Using an SD card for frequently changing data, such as databases, is not a good idea as it degrades the life of the card faster than expected. Instead, we should use external storage devices, such as hard drives or Network Attached Storage (NAS), to handle frequently changing data but also help with storing large amounts of media.

This book assumes that you are using at least an 8 GB SD card.

# Extra peripherals

The Raspberry Pi is branded as a computer, and it is expected that we can connect various different devices to it. Raspbian is based on Debian, and an immense effort has been made to port the majority of drivers and software available for Raspbian. You might have some old USB peripherals lying around, for example, a joystick. If you can find a driver for any other Linux platform, it should be possible to make it work with Raspbian. Plug it in, use the `lsusb` command-line utility, and check whether it has been detected. If you manage to get it working with your knowledge, you should share this knowledge on a forum for the benefit of other users.