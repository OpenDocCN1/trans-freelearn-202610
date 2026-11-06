In the GUI, add the directory on the hard drive. Now, install the application on your Mac OS X, Windows, Linux machine, or smart phone. Use the GUI to share directories between users. It can detect machines on the same network, which increases the speed of synchronization.

Sync tries its best to configure ports, and it uses protocols to communicate over the Internet. In theory, you can now call your friend on the phone, ask them to install Sync, and share the directory via an e-mail. They will now start seeing the same files as you.

![](bbox=[0.22, 0.36, 0.27, 0.41])

> BitTorrent Sync 2 uses external servers for tracking purposes, but no actual files are stored on them; there is no public access to the tracker files.
> 
> BitTorrent Sync claims to use SRP for mutual authentication and Perfect Forward Secrecy in which all data is then encrypted with AES-128 Counter Mode and unique session keys. It is a very complex way to ensure nobody can guess your key, not even BitTorrent. It is all dynamically generated on the fly, and then clients negotiate more keys during transfers. This sounds really good, but you will have to take them on their word as the project is closed source.

# The hardware RAID

The Raspberry Pi does not have a SATA controller on board, and there is no way to attach extra hard drives except via USB ports. The cool thing is that you can get a USB RAID controller. This makes file storage on the Raspberry Pi a very attractive option. Technically, the peripheral needed here is called the USB SATA multiplier.

# Configuration

There is no one-step guide to configuring these multipliers. Some need to be configured using software in Windows or Linux, while others may have DIP switches that configure the multiplier to a specific configuration.

Addonics is a well-known and fairly easy-to-source multiplier. You will need to search around on the Internet, online auctions, and shops for it. The prices are around the same as the Raspberry Pi but if you are looking for redundant storage, then these are the cheapest options you will find.