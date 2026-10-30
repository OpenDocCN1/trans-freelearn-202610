# Connecting Things to Your Pi with GPIO

Now, to turn the LED on and off: the GPIO pins are actually mapped as devices in the Linux file system, so using shell commands is easy, although there are many libraries available out there that allow you to control the GPIO using Python, for example. However, so that you don't have to learn a new language, we're going to do everything using shell commands.

The D0 pin that we are connected to is actually GPIO17 as far as the Raspberry Pi is concerned (take a look at the previous diagram for reference). The first thing we need to do is create file access to this GPIO pin. We do this with the following command:

```
```bash
sudo echo 17 > /sys/class/gpio/export
```
```

We then have to set the pin's direction to out:

```
```bash
sudo echo out > /sys/class/gpio/gpio17/direction
```
```

Next we can switch the pin on to turn the LED on:

```
```bash
sudo echo 1 > /sys/class/gpio/gpio17/value
```
```

To switch the LED off, we use this command:

```
```bash
sudo echo 0 > /sys/class/gpio/gpio17/value
```
```

Once we've finished with a GPIO port we can remove its file access:

```
```bash
sudo echo 17 > /sys/class/gpio/unexport
```
```

# Getting flashy…

We can put these commands together in a single Bash script to create a flashing LED. To create the flashy script, create a new text file in `nano` or some other text editor. Or, as I usually do (don't forget that I'm quite lazy), create the text file on your laptop, and then copy it to the remote Pi using **WinSCP** (although, read my note in the box that follows if you want to prevent some heartache).