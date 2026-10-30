![](bbox=[0.266, 0.148, 0.733, 0.351])

You'll also want to make sure you also get the connector cable for the device; it normally comes with the device. Here is a picture of the sensor with the cable attached:

![](bbox=[0.266, 0.432, 0.733, 0.635])

As noted in the tutorial, the voltage out of the sensor will be a voltage that will be an indication of the distance. However, this is an analog signal, and the Raspberry Pi doesn't have an analog-to-digital converter that can convert this analog voltage to a number that you can read in your program. You'll need to add an analog to digital converter to your project.