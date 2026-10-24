# Building and Controlling a Simple Wheeled Robot

In this program, the user determines the goal location and the robot decides on the shortest direction to the desired location by reading the angle. To make it simple, the robot is positioned in the grid, heading in the direction of an angle of 0 degrees. If the goal angle were less than 180 degrees, the robot would turn right. If it were greater than 180 degrees, the robot would turn left. The robot turns until the desired angle and the measured angle are within a few degrees of each other. Then, the robot takes the number of steps to reach the goal. As an additional challenge, you could add a loop to measure the actual angle and stop it when it reaches the target angle.

# # Summary

This chapter provided you with an opportunity to create a mobile platform for your Raspberry Pi Zero. You can add the sonar sensor or the compass from *Chapter 3, Accessing the GPIO Pins on Raspberry Pi Zero* to give it even more functionality. In the next chapter, you'll learn how to build a Raspberry Pi Zero platform robot with legs, an even more flexible mobile platform.