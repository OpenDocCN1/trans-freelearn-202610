# Project 2
The Stick Figure Dance Company

In Project 1, Romeo and Juliet we learned how to make Processing talk. In this project, we will learn how to make it see and dance. We will use Microsoft's Kinect to implement the seeing part and a dancing human to teach a group of stick figures how to dance. Unless you find someone else who is willing to lead your stick figures, you will have to get up and dance yourself.

Computer vision and 3D scanning have long been the domain of very specialized and expensive hardware and software. With the ever increasing computing power of CPUs and graphics cards, and new hardware controllers like Kinect, computer vision projects have invaded living rooms via various game controllers, and are accessible to everyday programmers like you and me.

# Mission Briefing

In this project, we will learn how to connect Microsoft's Kinect to a computer and use depth imaging and user tracking from Processing. We will rush through the installation of the OpenNI framework in the first task, as this library is used by the Processing library SimpleOpenNI. We will then learn how to use the depth image feature of the Kinect infrared camera and the player tracking function in Processing.

Then, we will use the so-called skeleton tracker, not only to locate the user in front of the camera, but also the head, neck, and elbows. These 3D coordinates will allow us to control a stick figure.

In the final task of our current mission, we are going to add a group of additional dancers that will also be controlled by the 3D coordinates of the players' limbs.