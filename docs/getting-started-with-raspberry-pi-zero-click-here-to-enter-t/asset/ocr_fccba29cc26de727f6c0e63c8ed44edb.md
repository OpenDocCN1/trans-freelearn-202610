You may want to add some heat-shrink tubing to cover your connections.

4. Finally, drill a hole in the back shell of the robot so that you can run both of these cables out of the unit, as shown in the following image:

![](bbox=[0.35, 0.22, 0.65, 0.52])

5. This picture also shows two more holes on either side of the shell, which you can use together with cable ties to attach the Raspberry Pi to the robot. Now you can put the shell back onto the robot.

6. Now, you need to connect these two wires to the Arduino. There are several versions of the Arduino that work but the most common one is the Arduino Uno. If you use a different Arduino with a different time, the delay constant in the Arduino code may need adjusting. The reason that you need to use an Arduino is that the bit patterns that are sent to the robot are sent at a fairly high rate and need to be created by a processor dedicated to this type of real-time communication. This allows the Raspberry Pi Zero to perform other processor-intensive activities, such as speech, while keeping the communication flowing at the correct rate.