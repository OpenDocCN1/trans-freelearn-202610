I am using a Logitech HD c310 USB Webcam as shown in the following image:

![](bbox=[0.3, 0.17, 0.7, 0.48])

> You can purchase it online, and you can find the product details and the specifications at http://www.logitech.com/en-in/product/hd-webcam-c310.

Attach your USB webcam to Raspberry Pi through the USB port on Pi and run the lsusb command in the terminal. This command lists all the USB devices connected to the computer. The output should be similar to the following output depending on which port is used to connect the USB webcam:

```
pi@raspberrypi ~/book/chapter04 $ lsusb
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
Bus 001 Device 004: ID 148f:2070 Ralink Technology, Corp. RT2070 Wireless Adapter
Bus 001 Device 007: ID 046d:081b Logitech, Inc. Webcam C310
Bus 001 Device 006: ID 1c4f:0003 SiGma Micro HID controller
Bus 001 Device 005: ID 1c4f:0002 SiGma Micro Keyboard TRACER Gamma Ivory
```