# # Creating a new layer

We will begin by creating a new layer. The new layer will act as the base of our home automation project. For now, it will only contain our image recipe, but we will add more content to the layer throughout the chapter until we have all the parts needed for our server. Just as described in Chapter 7, Deploying a Custom Layer on the Raspberry Pi, we will use yocto-layer to help us set up the structure of the layer. We need to set a high priority to ensure that content from this layer is prioritized over, for example, content in meta-raspberrypi, like so:

```
$ /path/to/poky/scripts/yocto-layer create pkt-iot
Please enter the layer priority you'd like to use for the layer: [default: 6] 10
Would you like to have an example recipe created? (y/n) [default: n]
Would you like to have an example bbappend file created? (y/n) [default: n]
New layer created in meta-pkt-iot.
Don't forget to add it to your BBLAYERS (for details see meta-pkt-iot/README).
```