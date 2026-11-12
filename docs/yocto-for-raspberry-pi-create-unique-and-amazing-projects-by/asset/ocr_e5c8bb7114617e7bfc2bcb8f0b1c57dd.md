Next, we can see that a gpio17 directory with some files has popped up in /sys/class/gpio:

```
$ ls /sys/class/gpio17/
```

After that, we need to set the direction (in/out) for the GPIO pin; in our example, we want the GPIO to act as an output pin:

```
$ echo "out" > /sys/class/gpio/gpio17/direction
```

Finally, we can enable the GPIO pin, which will cause the LED to turn on, by writing 1 to the value file:

```
$ echo 1 > /sys/class/gpio/gpio17/value
```

To turn off the light, we can write 0 to the value file:

```
$ echo 0 > /sys/class/gpio/gpio17/value
```

When done, you need to unexport the GPIO pin:

```
$ echo 17 > /sys/class/gpio/unexport
```

Now, the gpio17 directory in sysfs is gone. In our project, we will not manipulate the sysfs structure by hand. Instead, we will use the gpio-packet recipe added to our image earlier in this chapter, but under the hood, it uses the sysfs structure in a similar way as just described. The gpio-packet recipe produces a binary called gpio_example; it can be used to control GPIO17 using the following syntax:

```
$ gpio_example --led=1
$ gpio_example --led=0
```

Another way of controlling the GPIO pins is by using RPi.GPIO, which is a Python module that uses direct register access in the background to access GPIO. Controlling GPIO17 using RPi.GPIO requires that you add the following to a file called, for example, ledon.py:

```
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(17, GPIO.OUT)
GPIO.output(17, 1)
```

Then, we can run the script. Turning off the pin can easily be done by replacing 1 with 0 in the GPIO.output() function:

```
$ python ledon.py
```

RPi.GPIO can also use the physical GPIO header pin numbering. In that case, GPIO17 will be placed on pin 11. To use this mode, the script must be modified as follows:

```
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)
GPIO.output(11, 1)
```

# Note