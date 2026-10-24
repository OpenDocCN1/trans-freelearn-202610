
Also, before you start connecting wires, here is an image of the GPIO pins on the Raspberry Pi Zero board:

| Pin 1 3.3V |  | Pin 2 5V |
| Pin 3 GPIO2 |  | Pin 4 5V |
| Pin 5 GPIO3 |  | Pin 6 GND |
| Pin 7 GPIO4 |  | Pin 8 GPIO14 |
| Pin 9 GND |  | Pin 10 GPIO15 |
| Pin 11 GPIO17 |  | Pin 12 GPIO18 |
| Pin 13 GPIO27 |  | Pin 14 GND |
| Pin 15 GPIO22 |  | Pin 16 GPIO23 |
| Pin 17 3.3V |  | Pin 18 GPIO24 |
| Pin 19 GPIO10 |  | Pin 20 GND |
| Pin 21 GPIO9 |  | Pin 22 GPIO25 |
| Pin 23 GPIO11 |  | Pin 24 GPIO8 |
| Pin 25 GND |  | Pin 26 GPIO7 |
| Pin 27 ID_SD |  | Pin 28 ID_SC |
| Pin 29 GPIO5 |  | Pin 30 GND |
| Pin 31 GPIO6 |  | Pin 32 GPIO12 |
| Pin 33 GPIO13 |  | Pin 34 GND |
| Pin 35 GPIO19 |  | Pin 36 GPIO16 |
| Pin 37 GPIO26 |  | Pin 38 GPIO20 |
| Pin 39 GND |  | Pin 40 GPIO21 |

You need to connect these pins on the Raspberry Pi Zero GPIO to the pins on the H-bridge, as shown in the following table:

| Raspberry Pi Zero GPIO pin | H-bridge pin |
| 4 (5V) | 1 (Enable pin) |
| 13 (GPIO 27) | 2 (Forward) |
| 15 (GPIO 22) | 7 (Backward) |
| 4 (5V) | 11 (Enable 2) |
| 38 (GPIO 6) | 10 (Forward) |
| 40 (GPIO 13) | 15 (Backward) |
| 6 (GND) | 4, 5, 12, 13 (GND) |
| 2 (5 Volts) | 16 (VCC) |
| Battery positive terminal | 8 (Vc) |
| Battery negative terminal | GND (connect to the same GND as previous GND pins) |

Once you have the connections, you can test the system. To make this all work, you need to add some code, which we will see in the next section.

## Controlling your mobile platform programmatically using the Raspberry Pi Zero

Now that you have your basic motor controller functionality up and running, you need to connect both motor controllers to the Raspberry Pi Zero. This section will cover this and also show you how to control your entire platform programmatically.

You are going to use Python in your initial attempts to control the motor. It is very straightforward to code, run, and debug your code in Python. The first Python program you are going to create is shown in the following screenshot:

![](img/324d126a7e2da696837a3e066326bd61_99_0.png)

Perform the following steps to create this program:

To create this program, create a directory called dcmotor in your home directory by typing mkdir dcmotor and then type cd dcmotor. Now, open the file by typing emacs dcmotor.py (if you are using a different editor, open a new file with the dcmotor.py name).

Now, enter the program. Let's go through the program step by step:

- #!/usr/bin/python: This line lets you run the program without having to type python before the filename. You'll learn how to do this at the end of these instructions.
- import RPi.GPIO as io: This lets you import the RPi library, which allows you to control the GPIO pins.
- io.setmode(io.BCM): This sets the specification mode of the GPIO pins to Broadcom SOC channel (BCM) number. This means that you will specify the GPIO numbers of the pins you want to control instead of the actual physical pin values.
- in1_pin1 = 27: This assigns the value 27 to the in1_pin1 variable.
- in2_pin1 = 22: This assigns the value 22 to the in1_pin1 variable.
- in1_pin2 = 20: This assigns the value 20 to the in1_pin1 variable.
- in2_pin2 = 21: This assigns the value 21 to the in1_pin1 variable.
- io.setup(in1_pin1, io.OUT): This sets GPIO pin 27 to an output control.
- io.setup(in2_pin1, io.OUT): This sets GPIO pin 22 to an output control.
- io.setup(in1_pin2, io.OUT): This sets GPIO pin 20 to an output control.
- io.setup(in2_pin2, io.OUT): This sets GPIO pin 21 to an output control.
- def forward(): This defines the forward function. You have to turn on GPIO 27 and GPIO 20 and turn off GPIO 22 and GPIO 21.
- io.output(in1_pin1, True): Output a 3.3 volt signal on in1_pin1 (this is GPIO 27).
- io.output(in2_pin1, False): Output 0 volts on in2_pin1 (this is GPIO 22).
- io.output(in1_pin2, True): Output a high voltage on in1_pin2 (this is GPIO 20).
- io.output(in2_pin2, False): Output 0 volts on in2_pin2 (this is GPIO 21).
- def reverse(): This defines the reverse function. You'll turn on GPIO 22 and GPIO 21, and turn off GPIO 27 and GPIO 20.
- io.output(in1_pin1, False): Output 0 volts on in1_pin1 (this is GPIO 27).
- io.output(in2_pin1, True): Output a high voltage on in2_pin1 (this is GPIO 22).
- io.output(in1_pin2, False): Output 0 volts on in1_pin2 (this is GPIO 20).
- io.output(in2_pin2, True): Output a high voltage on in2_pin2 (this is GPIO 21).
- def stop(): This defines the stop function. You set the level to 0 on the pins off GPIO 22, GPIO 21, GPIO 27, and GPIO 20.
- io.output(in1_pin1, False): Output 0 volts on in1_pin1 (this is GPIO 27).
- io.output(in2_pin1, False): Output 0 volts on in2_pin1 (this is GPIO 22).
- io.output(in1_pin2, False): Output 0 volts on in1_pin2 (this is GPIO 20).
- io.output(in2_pin2, False): Output 0 volts on in2_pin2 (this is GPIO 21).
- while True: This performs loops over and over. You can stop the program by pressing Ctrl + C.
- cmd = raw_input("Enter f (forward) or r (reverse) or s (stop): "): Enter a character for what you want the robot to do.
- direction = cmd[0]: Take only the first character of the input.
- if direction == "f": If the direction is "f", then execute the next statement.
- forward(): Execute the forward function.
- if direction == "r": If the direction is "f", then execute the next statement.
- reverse(): Execute the reverse function.
- if direction == "s": If the direction is "f", then execute the next statement.
- stop(): Execute the stop function.

![](img/324d126a7e2da696837a3e066326bd61_103_0.png)

The following is an explanation of the lines of code that you just added:

- io.setup(in2_pin1, io.OUT): This sets GPIO 27 to an output.
- p1 = io.PWM(in1_pin1, 50): Instead of just on or off settings, this PWM setting allows the programmer to set the relative width of the pulse. This initializes this functionality on GPIO 27.
- p1.start(0): This starts the pulses on p1, GPIO 27, with a pulse width of 0 percent, or off.
- io.setup(in2_pin1, io.OUT): This sets GPIO 22 to an output.
- p2 = io.PWM(in2_pin1, 50): This initializes this functionality on GPIO 22.
- p2.start(0): This starts the pulses on p2, GPIO 22, with a pulse width of 0 percent, or off.
- io.setup(in1_pin2, io.OUT): This sets GPIO 20 to an output.
- p3 = io.PWM(in1_pin2, 50): This initializes this functionality on GPIO 20.
- p3.start(0): This starts the pulses on p3, GPIO 20, with a pulse width of 0 percent, or off.
- io.setup(in2_pin2, io.OUT): This sets GPIO 21 to an output.
- p4 = io.PWM(in2_pin2, 50): This initializes this functionality on GPIO 21.
- p4.start(0): This starts the pulses on p3, GPIO 21, with a pulse width of 0 percent, or off.
- def forward(50): This function moves the unit forward by setting the pulse width in a forward direction to 50 percent.
- p1.start(50): This sets the value of p1 (GPIO 27) to 50 percent on and 50 percent off. This should result in the motor running forward at half speed.
- p2.start(0): This sets the value of p2 (GPIO 22) to 0 percent. This effectively turns this pin off.
- p3.start(50): This sets the value of p3 (GPIO 20) to 50 percent on and 50 percent off. This should result in the motor running forward at half speed.
- p4.start(0): This sets the value of p4 (GPIO21) to 0 percent. This effectively turns this pin off.
- def reverse(50): This function moves the unit in reverse by setting the pulse width in the reverse direction to 50 percent.
- p1.start(0): This sets the value of p1 (GPIO 27) to 0 percent. This effectively turns this pin off.
- p2.start(50): This sets the value of p2 (GPIO 22) to 50 percent on and 50 percent off. This should result in the motor running in reverse at half speed.
- p3.start(0): This sets the value of p3 (GPIO 20) to 0 percent. This effectively turns this pin off.
- p4.start(50): This sets the value of p4 (GPIO21) to 50 percent on and 50 percent off. This should result in the motor running in reverse at half speed.
- def stop(): This function sets all PWM signals to 0 percent, effectively stopping the motors.
- p1.start(0): This sets the value of p1 (GPIO 27) to 0 percent. This effectively turns this pin off.
