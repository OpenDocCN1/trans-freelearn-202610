# Objective Complete - Mini Debriefing

We have just learned how to access the depth image provided by Kinect using Processing. Starting with step 9, we used the tracking functions of the SimpleOpenNI framework. We added a callback function named onNewUser(), which gets called by the library every time a user is detected, and a second one named onLostUser() to receive a notification if the user isn’t in the tracking range anymore.

In our draw() method, we used a pixel map that we got from the getUsersPixels() method to color all the pixels in the depth image green if they belong to the user and leave them unchanged if they don’t. The array returned by the function getUsersPixels() contains a value of 0 for every non-user pixel and a 1 for the ones we want to color. For every 1 that is part of the user’s pixels, we take the current gray value of the depth image and replace it with a green value of the same intensity.

# Classified Intel

Apart from the infrared and RGB camera, the Kinect also comes with a motor control to allow for the adjustment of the camera tilt. Unfortunately, the OpenNI framework has no support for controlling the motor. There is a second open source framework called libfreenect, which can be used to access the Kinect. It lacks the user-tracking features of OpenNI but comes with support for motor control. So if you need to adjust the tilt level of your Kinect, because it’s notorious for cutting off the heads or feet of your players, you can install the libfreenect library and the demo applications and use them to adjust your Kinect.

# Making a dancer

In the previous section, we used the user-tracking capabilities of the OpenNI framework to locate the user in the depth image provided by the Kinect infrared camera. Now we will take it one step further and locate the body parts of the player. The feature we are going to use in this task is called skeleton tracking. The OpenNI skeleton tracker locates certain key points of a human body, which we will use to construct our stick figure. For each player, the Kinect can see and get the location of the head, neck, torso, shoulders, elbows, hands, hips, knees, and feet.

We are also showing the image of the infrared camera and the user pixels we used in the last section as a little heads-up display (HUD) so that the player is able to see what Kinect is tracking.