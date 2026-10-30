# Introduction to GPIO Programming

GPIO.setmode() sets the addressing mode of the GPIO pins. There are two modes, namely BOARD and BCM. The BOARD mode addresses the pins by the numbering given on the board. For example, in the preceding GPIO diagram, the GPIO04 is pin 7. The other mode is BCM and it defines the pins according to the Broadcom Firmware on the Raspberry Pi. So, the GPIO04 pin will be referred to as 4 rather than 7. The next statement, GPIO.setup(), configures the specified pin to be an output or input depending on the argument.

Now, we'll move on to the main chunk of the code, which is the Blink() function. The GPIO.output() function takes two arguments: the pin number and the logical operation to be performed. In the first output statement, we set pin 7 to HIGH, and then in the second output statement, we set it to LOW. In between these functions, we have the sleep() function from the time package, which takes only one argument: the time in seconds. It does nothing but delay the execution of the next statement by the input number of seconds, which could also be fractions such as 0.5 or 0.2. The last statement in the Blink() function is the cleanup() function, which resets all the ports you have used back to the input mode. This prevents the pin from being damaged when you have set HIGH as an output and then accidentally connect it to ground, which would short-circuit the port. So, it's safer to leave the ports as inputs.

# Connecting a button

As we saw in the previous section, connecting an LED to a GPIO pin was easy, and the code was simple. In this section, we will use the same general code in order to program a GPIO pin to take an input from the button and if the button is pressed, light the LED that we have already connected in the previous section. We will now take a look at the code and then explain those lines that were not encountered in the previous section:

```
```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BOARD)

GPIO.setup(12, GPIO.IN, pull_up_down = GPIO.PUD_UP)
GPIO.setup(7, GPIO.OUT)

while True:
    input_state = GPIO.input(12)

    if input_state == False:
        print('Button Pressed')
    GPIO.output(7, True)
```
```