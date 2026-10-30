# Using a servo controller to control the servos

To make your biped walk, you first need to connect the servo motor controller to the servos. The servo controller you are going to use for this project is a simple servo motor controller utilizing the USB from Pololu – Pololu item number 1354 is available at pololu.com – that can control 18 servo motors. Here is a picture of the unit:

![](bbox=[0.26, 0.3, 0.74, 0.51])

Make sure that you order the assembled version. This piece of hardware will turn USB commands from Raspberry Pi into signals that control your servo motors. Pololu creates a number of different versions of this controller, and each one is able to control a certain number of servos. In this case, you may want to choose the 18 servo version, so that you can control all 12 servos with one controller, and you may also add an additional servo to control the direction of a camera or sensor. You could also choose the 12 servo version. One advantage of the 18 servo controller is the ease of connecting power to the unit via screw-type connectors.

There are two connections you'll need to make to the servo controller in order to get started: the first to the servo motors and the second to a battery.