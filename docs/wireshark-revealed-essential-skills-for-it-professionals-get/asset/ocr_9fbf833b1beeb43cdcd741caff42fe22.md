will explain the procedure so that you can figure out the most commonly seen error code, which is client errors.

The infrastructure I am going to use is pretty simple, easy, and similar to the one that we used earlier. The client is located at 192.168.10.196 and the gateway is located at 192.168.10.1. I will try to make a few requests to the gateway and a few to any web server located in the wild (note that my intention is just to replicate error code that you can see in the list pane of Wireshark, and not to compromise any web server.)

![](bbox=[0.07, 0.25, 0.93, 0.4])

At first, we will try to generate some client error code. Follow the next steps to walk through this; otherwise, you can just read it once and then replicate the whole scenario:

+   1. Open your browser and visit the default home page of your gateway. Hopefully, it will present you with a login screen like the one shown here: