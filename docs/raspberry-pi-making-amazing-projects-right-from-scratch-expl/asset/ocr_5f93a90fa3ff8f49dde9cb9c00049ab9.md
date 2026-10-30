# Avoiding Obstacles Using Sensors

2. Now that you know the pins you'll connect to, you can connect the sonar sensor. There is a problem, as you can't connect the 5 volt return from the sonar sensor directly to the Raspberry Pi GPIO pins; they want 3.3 volts. You'll need to build a voltage divider that will reduce the 5 volts to 3.3 volts. This can be done with two resistors, which are connected as shown in this diagram:

![](bbox=[0.314, 0.239, 0.686, 0.388])

If you'd like more information on how the voltage divider works in this configuration, refer to http://www.modmypi.com/blog/hc-sr04-ultrasonic-range-sensor-on-the-raspberry-pi. The combination of these two resistors will reduce the voltage to the desired levels. You may want to put all of this is in a small breadboard, as shown here:

![](bbox=[0.264, 0.503, 0.735, 0.776])