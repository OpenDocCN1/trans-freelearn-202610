If you don't, you can easily create this sort of overhead support using a PVC pipe or wood. Really, just something to keep your biped from crashing over during your experimentation.

# A basic stable pose

Now that your biped is built and you know how to program the servos using Python, you can experiment with some basic poses. You'll first create a program that allows you to set individual servos so that you can experiment. Here is the program:

![](bbox=[0.23, 0.31, 0.77, 0.7])

This code includes the Python setAngle function from Chapter 2, Building the Biped. The specifics were taken from the www.pololu.com website, but it simply allows you to set a specific servo to a specific angle.

The next part of the code sets all of the servos to their center location. The final piece of the code, the while 1: code set, simply asks the user for a servo and an angle, and then sends the command to the servo controller.