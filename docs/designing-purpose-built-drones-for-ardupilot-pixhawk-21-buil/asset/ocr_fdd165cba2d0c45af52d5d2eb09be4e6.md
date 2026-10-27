# The final plan

It's very important with a complex build like this to do more than just a sketch for how everything will lay out. Too much money is on the line to leave it to a sketch. The following image shows a three-quarter view of our final plan:

![](bbox=[0.27, 0.26, 0.78, 0.38])

# Implementing Pixhawk

In previous iterations of this type of hybrid, you would need to use PX4 firmware (rather than Ardupilot or APM) with heavy customization. Luckily, the newer Ardupilot firmware versions allow for exactly this type of VTOL (they call it a Quadplane). Obviously, the channels are going to be a little different.

The following diagram shows how all of the motors/servos are hooked up:

![](bbox=[0.21, 0.62, 0.81, 0.835])