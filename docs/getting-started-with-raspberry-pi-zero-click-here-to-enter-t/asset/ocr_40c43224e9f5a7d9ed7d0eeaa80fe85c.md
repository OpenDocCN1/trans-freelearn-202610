+   - `p2 = io.PWM(in2_pin1, 50)`: This initializes this functionality on GPIO 22.
+   - `p2.start(0)`: This starts the pulses on p2, GPIO 22, with a pulse width of 0 percent, or off.
+   - `io.setup(in1_pin2, io.OUT)`: This sets GPIO 20 to an output.
+   - `p3 = io.PWM(in1_pin2, 50)`: This initializes this functionality on GPIO 20.
+   - `p3.start(0)`: This starts the pulses on p3, GPIO 20, with a pulse width of 0 percent, or off.
+   - `io.setup(in2_pin2, io.OUT)`: This sets GPIO 21 to an output.
+   - `p4 = io.PWM(in2_pin2, 50)`: This initializes this functionality on GPIO 21.
+   - `p4.start(0)`: This starts the pulses on p3, GPIO 21, with a pulse width of 0 percent, or off.
+   - `def forward(50)`: This function moves the unit forward by setting the pulse width in a forward direction to 50 percent.
+   - `p1.start(50)`: This sets the value of p1 (GPIO 27) to 50 percent on and 50 percent off. This should result in the motor running forward at half speed.
+   - `p2.start(0)`: This sets the value of p2 (GPIO 22) to 0 percent. This effectively turns this pin off.
+   - `p3.start(50)`: This sets the value of p3 (GPIO 20) to 50 percent on and 50 percent off. This should result in the motor running forward at half speed.
+   - `p4.start(0)`: This sets the value of p4 (GPIO21) to 0 percent. This effectively turns this pin off.
+   - `def reverse(50)`: This function moves the unit in reverse by setting the pulse width in the reverse direction to 50 percent.
+   - `p1.start(0)`: This sets the value of p1 (GPIO 27) to 0 percent. This effectively turns this pin off.
+   - `p2.start(50)`: This sets the value of p2 (GPIO 22) to 50 percent on and 50 percent off. This should result in the motor running in reverse at half speed.
+   - `p3.start(0)`: This sets the value of p3 (GPIO 20) to 0 percent. This effectively turns this pin off.
+   - `p4.start(50)`: This sets the value of p4 (GPIO21) to 50 percent on and 50 percent off. This should result in the motor running in reverse at half speed.
+   - `def stop()`: This function sets all PWM signals to 0 percent, effectively stopping the motors.
+   - `p1.start(0)`: This sets the value of p1 (GPIO 27) to 0 percent. This effectively turns this pin off.