# 3

# Extending Your Pi to Connect More Things

We're now going to look at ways to expand the number of things we can connect to our Raspberry Pi, overcoming the limitation of having just the 8 digital pins available. We're going to do this by building our own expansion board to give us what could in theory be an unlimited number of digital inputs and outputs.

We're also going to overcome the limitations of the +3.3V power available to us by building our own +3.3V power supply that taps off the Raspberry Pi's +5V supply.

In this chapter, we will cover the following:

+   - Looking at the I²C bus in detail
+   - Learning about serial-to-parallel and parallel-to-serial conversions
+   - Building a +3.3V power supply
+   - Building an I2C-based port expander to give us more inputs and outputs
+   - Looking at alternative ready-made expansion boards

# Prerequisites

Along with your Raspberry Pi, you'll need the following parts for the projects in this chapter:

+   - A copper strip board (or Veroboard®)
+   - An LD1117V33 voltage regulator
+   - A 2 x 100nF, 16V ceramic capacitor
+   - A 10uF, 16V electrolytic capacitor