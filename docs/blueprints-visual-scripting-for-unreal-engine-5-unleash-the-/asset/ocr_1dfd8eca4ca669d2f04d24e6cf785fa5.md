+   - What direction it is supposed to move in?
+   - How fast it should move?
+   - When should it switch between directions?

We will get the current location by using a function that returns the coordinates of the Cylinder in the world. The speed, direction, and time to change direction are the values we are providing to the Blueprint, but some calculations are necessary to turn those values into information that the Blueprint can use to move the object.

Follow these steps:

+   1. In the **My Blueprint** panel, create a variable named `speed`. This variable will hold a number that represents the speed of movement of `BP_CylinderTarget`. In the **Details** panel, change **Variable Type** to **Float** and check the **Instance Editable** attribute:

![](bbox=[0.13, 0.48, 0.7, 0.76])

Figure 5.24 – The Speed variable

+   2. Compile the Blueprint to be able to set the default value. Set **DEFAULT VALUE** to 200.0: