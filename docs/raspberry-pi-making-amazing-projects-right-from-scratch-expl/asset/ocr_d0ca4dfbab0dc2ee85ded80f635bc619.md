The MCP23017 has been given its unique base address, but can be modified by changing the address pins A0-A2 high or low; thus, in effect, it can be configured to be one of 8 addresses. This is why you can only have a maximum of 8 of these chips on the same I2C bus, giving us a theoretical maximum of 128 I/O pins (that is, 16 I/Os x 8 chips).

# Building your expansion board

This circuit can easily be built on a small piece of stripboard. The following image shows an example of the layout, which looks a bit simpler than the circuit diagram. In the next chapter, we'll learn how to connect up our board and program it so we can check that it works.

![](bbox=[0.255, 0.35, 0.75, 0.64])