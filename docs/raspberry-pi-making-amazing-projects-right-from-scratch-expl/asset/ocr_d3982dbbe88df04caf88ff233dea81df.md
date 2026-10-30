The advantages are as follows:

+   - Their ease and speed of installation
+   - Their ease of removal, which means that you can take them anywhere with you
+   - Expanding the system in the future can be easier, with most systems automatically detecting new units

The disadvantages are as follows:

+   - They are more expensive than wired systems, sometimes three or four times the cost
+   - They are not as secure as wired systems and cannot achieve a security grading greater than two in accordance with European Standard BSEN 50131 (although, this grade is suitable for domestic properties)
+   - Wireless devices need to have their batteries replaced at regular intervals
+   - Wireless systems are less reliable and susceptible to interference and even radio jamming

# Connecting a 433-MHz receiver

In the past, it was possible to roll out your own 433-MHz receiver for the Raspberry Pi using an inexpensive receiver, such as the XY-MV-5V module along with the 433-Util library that was put together by a guy called Mark Wolfe, a contributor on GitHub. Essentially, he gathered together code relating to 433-MHz communications and put it all into this library. Originally developed for Arduino, this has now been ported to the Raspberry Pi.

You can then use a readily available transmitter, such as a key fob or any other 433-MHz transmitter, and take a look at the incoming code as you press each button on the transmitter.

![](bbox=[0.35, 0.64, 0.64, 0.8])

A XY-MK-5V Generic 433-MHz receiver module