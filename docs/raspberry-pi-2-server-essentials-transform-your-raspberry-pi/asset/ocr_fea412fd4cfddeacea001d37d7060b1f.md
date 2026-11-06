# 2
Preparing a Network

It is important to learn how your network works, especially if you plan to connect your Raspberry Pi to the Internet. A home user will typically use an Internet package designed to browse websites and read e-mails. Business packages, on the other hand, need to do a lot more than you think. These two different ISP packages usually carry important technical differences that dictate how your network can be reached.

In this chapter, you will learn how to connect to the Internet and look at how to solve some common problems for home users. You will also learn how to benchmark your network and try to isolate any network-related issues.

# Local Area Network (LAN)

Using the standard Raspbian package, both essential and non-essential drivers are included. All essential drivers are loaded and some non-essential ones are as well.

We will start by plugging in a network cable between the Raspberry Pi and the router provided by your ISP. By default, your router has a DHCP server that automatically assigns an IP address to your Raspberry Pi.

You may also use network switches to make a more complex network but because the Raspberry Pi has a 100 megabit network port, it may downgrade your entire network to 100 megabits. Some switches can negotiate separate connections to lower speed interfaces without downgrading the entire network, but you need to consult the specifications of the device in order to do this.