
## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning.

Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: “In the setup() function of the sketch, we will try to get an IP address using DHCP.”

A block of code is set as follows:

```
String log_time = String(day()) + "/" +
String(month()) + "/" + String(year()) + " " +
String(hour()) + ":" + String(minute()) + ":" +
String(second));
```

Any command-line input or output is written as follows:

```
# 192.168.1.103/digital/7/1
```

New terms and important words are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: “If you are using Windows, you will find the information you need under Network Settings in your Control Panel.”

## Note

Warnings or important notes or some additional information appear in a box like this.

## Tip

Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book—what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an e-mail to <feedback@packtpub.com>, and mention the book title through the subject of your message.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on www.packtpub.com/authors.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for all Packt books you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

All the up-to-date code for the projects of this book can also be found at the following address:

https://github.com/openhomeautomation/arduino-networking/

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the **errata submission form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website, or added to any list of existing errata, under the Errata section of that title.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at <copyright@packtpub.com> with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at <questions@packtpub.com> if you are having a problem with any aspect of the book, and we will do our best to address it.

## Chapter 1. Discover the Arduino Ethernet Shield

In this first chapter of the book, we will only focus on the basics and get started with the Arduino Ethernet shield. In order to build more complex projects in the next chapters of the book, we first need to be absolutely sure that our shield is functioning correctly, and it can connect to our local network and the Web.

To ensure this, we’ll first assemble the hardware, and then build a simple Arduino sketch that will connect to the Web, grab a test web page, and display it back inside the Arduino Serial Monitor.

These will be the major takeaways of this chapter:

- First, we’ll make sure that you have all the required hardware and software components. We are also going to assemble the shield and the Arduino Uno board, and connect everything to our local network via a router.
- Then, we will write the sketch that will connect your shield to your local network as well as the Web, and this sketch will grab a test page to make sure your connection is working correctly.
- Finally, we are going to test this sketch and monitor the status of the Ethernet shield on the Arduino Serial Monitor. From this first test of the shield, we’ll be able to tell whether the shield is correctly connected to the Internet or not.

### Hardware and software requirements

You don’t need a lot of hardware for this first project. You only need an Arduino Uno board, and of course, the Arduino Ethernet shield. Other boards such as the Arduino Mega will work as well for the projects in the first chapter of the book, but you might face difficulties for the projects in the following chapters in the book. Therefore, I recommend that you use an Arduino Uno board for all the projects of this book.

Before actually assembling the shield with the board, make sure to write down the MAC address of the shield, which is written on the back of the shield. We will need it later in the project when we write the first sketch.

This is an image of the assembled board and the Ethernet shield:

![](img/b454031c864977fe997c5feb0b3f9b63_42_0.png)

You will also need a USB B to USB A cable to connect the Arduino board to your computer, and an Ethernet cable to connect the shield to a router.

On the software side, all you need is the Arduino IDE, which can be found at the following address:

http://arduino.cc/en/main/software

The Ethernet library is included by default in the Arduino software, so you don’t need to install any additional components.

### Hardware configuration

The hardware configuration in this chapter is really simple. At this point, you should already have the Arduino Ethernet shield connected to your Arduino Uno board. If that's not the case, please do so now.

You then have only two cables to plug in: a USB cable between your computer and the Arduino Uno board and the Ethernet cable. Simply connect the USB cable to one port of your computer.

For the Ethernet cable, you have several options. The easiest thing you can do, which is what I recommend, is to connect the Ethernet cable directly from the shield to the main router of your home. Usually, you will have a Wi-Fi router in your home, which you use to enable Wi-Fi connectivity for your computer and other devices. This router should also have some Ethernet ports, where you can connect your Ethernet shield. The advantage of this solution is that your shield will automatically get an IP address and will have access to the Internet automatically. This assumes that your router is configured for DHCP; go to your router settings to enable DHCP. As this procedure varies depending on the brand of your router, refer to your router documentation to find out how to do this on your specific router.

If you don't have a router, you can directly connect the Ethernet cable to your computer. I don't recommend this alternative, as configuring your computer to connect to the shield and sharing the Internet connection with the shield is really complex, and it also depends on your operating system.

If you really don't have access to a router but you have access to a Wi-Fi network, I recommend buying one of these small Wi-Fi routers that have an Ethernet port. It can connect to any Wi-Fi network in range and share the connection automatically with other devices via Ethernet.

For example, as shown in the following image, I recommend the TP-Link MR3020 router:

![](img/b454031c864977fe997c5feb0b3f9b63_43_0.png)

This is an image of the assembled hardware for the tasks in this chapter, with the USB
