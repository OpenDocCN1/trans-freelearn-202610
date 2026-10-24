
## Creating a program in Linux to control your quadruped

You now know that you can talk to your servo motor controller and move your servos. In this section, you'll create a Python program that will let you talk to your servos to move them to specific angles.

Let's start with a simple program that will make your legged mobile robot's servos go to 90 degrees (which should be somewhere close to the middle of the 0 to 180 degrees you can set.) To access the serial port, you'll need to make sure that you have the Python serial library. If you don't, then type sudo apt-get install python-serial. After you have installed the serial library, you can run your program by typing sudo python quad.py.

This particular controller uses two bytes of information, so the code in the setAngle function will translate the input of the channel and angle into numbers that the controller can understand. For more specifics, see http://www.pololu.com/docs/0J40. Here is the code:

![](img/324d126a7e2da696837a3e066326bd61_134_0.png)

Here is an explanation of the code:

- #!/usr/bin/python: This first line allows you to execute this Python file from the command line.
- `import serial`: This line imports the `serial` library. You need the `serial` library to talk to your unit via USB.
- `def setAngle(ser, channel, angle)`: This function converts your desired setting for servo and angle into the serial command that the servo motor controller needs.
- `ser = serial.Serial("/dev/ttyACM0", 9600)`: This opens the serial port connection to your servo controller.
- `for i in range(0, 12)`: This `for` loop will access each servo.
- `setAngle(ser, i, 90)`: Now, you can set each servo to the middle (home) position. The default would be to set each servo to 90 degrees. If your legs aren't in their middle position, you can adjust them by adjusting the position of the servo horns on each servo.

Once you have the basic home position set, you can now ask your robot to do things. Let's start by making your quadruped wave. Here is the Python code:

![](img/324d126a7e2da696837a3e066326bd61_135_0.png)

In this case, you are using the setAngle command to set your servos to manipulate your robot's front-right arm. The middle servo raises the arm and the lower servo then goes back and forth between angle 100 and 130 degrees.

One of the most basic actions you'll want your robot to take is to walk forward. Here is an example of how to manipulate the legs to make this happen:

```
ser = serial.Serial("/dev/ttyACM0", 9600)
# Home position
for i in range(0, 12):
    setAngle(ser, i, 90)

setAngle(ser, 4, 110)
time.sleep(1)
setAngle(ser, 5, 100)
time.sleep(1)
setAngle(ser, 4, 90)
time.sleep(1)

setAngle(ser, 7, 70)
time.sleep(1)
setAngle(ser, 8, 80)
time.sleep(1)
setAngle(ser, 7, 90)
time.sleep(1)

setAngle(ser, 1, 110)
time.sleep(1)
setAngle(ser, 2, 100)
time.sleep(1)
setAngle(ser, 1, 90)
time.sleep(1)

setAngle(ser, 10, 70)
time.sleep(1)
setAngle(ser, 11, 80)
time.sleep(1)
setAngle(ser, 10, 90)
time.sleep(1)

for i in range(0, 12):
    setAngle(ser, i, 90)

ser.close()
```

This program lifts and then moves each leg forward, one at a time, then moves all the legs to the home position, which moves the robot forward. Not the most elegant, but it does work. There are more sophisticated algorithms for walking with your quadruped; see http://letsmakerobots.com/node/35354 and https://www.youtube.com/watch?v=jWP3RnYa_tw. Once you have the program working, you'll want to package all your hardware onto the mobile robot.

You can make your robot do many amazing things, such as walk forward, walk backward, dance, and turn around; any number of movements are possible. The best way to learn is to try new and different positions with the servos.

## Summary

You now have a robot that can walk! You can also add other sensors, such as the ones you discovered for your wheeled robot, sensors that can watch for barriers, or others that know the direction you are moving in.

In the next chapter, you'll start on a new robot. You'll combine a Raspberry Pi Zero with a toy robot and construct a project with the ability to speak and respond to voice commands.

## 6
Adding Voice Recognition and Speech – A Voice Activated Robot

You've started with some pretty basic projects but you can take the concepts you have learned further by modifying robotic toys with the Raspberry Pi Zero. One class of toys that are excellent candidates for your projects are a set of robot toys by WowWee. You can purchase these toys from the company at http://wowwee.com/ but you can also find used versions of these toys on eBay for a significantly lower price.

In this chapter, you'll learn the following:

- How to break into the toy and provide the control signals for your project
- How to send and receive voice commands to control the robot
- How to interpret commands and initiate actions

There are several toys that have excellent possibilities. One such toy is the **WowWee Roboraptor**. The following picture shows this robot:

![](img/324d126a7e2da696837a3e066326bd61_140_0.png)

Another option is the **WowWee Robosapien**. Here is a picture of this one:

![](img/324d126a7e2da696837a3e066326bd61_140_1.png)

We will use this robot for the project, as it has more functionality and is easier to modify.

## Communication between the Raspberry Pi Zero and the robot

You're going to connect to the internal serial bus so that you, not the remote, can send commands. You'll add an Arduino to handle real-time communication between the Raspberry Pi Zero and the robot. Here are the steps:

- 1. Firstly, you need to disassemble the robot to get access to the main controller board. To do this, lay the robot face down so that you have access to the back. Remove the plate at the back by unscrewing the four screws that hold it in place. Now, at the top of the exposed board, you should see the main connector. The following image is a close-up photograph of the connector:

![](img/324d126a7e2da696837a3e066326bd61_141_0.png)

There are only two wires that you are interested in. The first is the black wire, which is the GND for the Robosapien system. The second is the white wire. This is the serial connection that controls the command for the Robosapien.

You may want to add some heat-shrink tubing to cover your connections.

- 4. Finally, drill a hole in the back shell of the robot so that you can run both of these cables out of the unit, as shown in the following image:

![](img/324d126a7e2da696837a3e066326bd61_143_0.png)

- 5. This picture also shows two more holes on either side of the shell, which you can use together with cable ties to attach the Raspberry Pi to the robot. Now you can put the shell back onto the robot.

- 6. Now, you need to connect these two wires to the Arduino. There are several versions of the Arduino that work but the most common one is the Arduino Uno. If you use a different Arduino with a different timer, the delay constant in the Arduino code may need adjusting. The reason that you need to use an Arduino is that the bit patterns that are sent to the robot are sent at a fairly high rate and need to be created by a processor dedicated to this type of real-time communication. This allows the Raspberry Pi Zero to perform other processor-intensive activities, such as speech, while keeping the communication flowing at the correct rate.

For specifics on how to use the development environment of the Arduino family of processors, visit https://www.arduino.cc. For information on how to use the Raspberry Pi to create and upload programs to the Arduino, refer to http://www.instructables.com/id/Arduino-On-Pi/ or https://www.youtube.com/watch?v=wCxQrW96jTM.

- 7. Connect the GND wire to one of the GND pins on the Arduino. Then, connect the other wire to pin 9 on the Arduino. These connections should look similar to the following picture:

![](img/324d126a7e2da696837a3e066326bd61_144_0.png)

- 8. The final step is to get the code to send the correct commands to the Arduino board. The code for this book is available in the PacktPub code download section or at http://playground.arduino.cc/Main/RoboSapienIR. This Arduino code takes an input from the Serial Monitor, in this case a USB connection from the Raspberry Pi Zero and turns it into the appropriate command for the WowWee robot. Once you have uploaded the code to the Arduino, either by using an external PC or a Raspberry Pi Zero, you can use the Arduino IDE's Serial Monitor capability to send individual letter commands and the robot should respond to these commands.
