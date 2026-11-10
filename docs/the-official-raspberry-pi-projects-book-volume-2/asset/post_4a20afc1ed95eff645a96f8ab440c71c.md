
## Tutorial
## WALKTHROUGH
## EVERYDAY ENGINEERING PART 12
## SIMON MONK

Simon Monk is the author of the Raspberry Pi Cookbook and Programming the Raspberry Pi: Getting Started with Python, among others.
simonmonk.org
monkmakes.com

![](img/d8322379620245fcd21eccc3855b8c78_137_0.png)

## BINARY
## CLOCK

### You'll Need

- Raspberry Pi with 40-pin GPIO header
- Sense HAT or Pimoroni Unicorn HAT
- USB WiFi adaptor or Ethernet connection

Solve real-world electronic and engineering problems with your Raspberry Pi and the help of renowned technology hacker and author, Simon Monk

This is a very simple project to make. In fact, all you really need to do is plug the Sense or Unicorn HAT onto the Raspberry Pi’s GPIO header and install the software.

This project displays the current time in binary. The rows of the display are (from top to bottom) the last two digits of the year (e.g. 15), the month number, the day of the month, the hour (24-hour format), the minute, the second, and finally hundredths of a second.

### Reading the binary clock

To read a binary clock, you either need to be good at recognising binary numbers or use a bit of arithmetic to calculate the time, by adding up the binary digit values for the LEDs that are lit on a particular row.

For example, the first row displays the year in two-digit format. So, if LEDs at positions 8, 4, 2, and 1 are all lit, then the year is 8+4+2+1 = 15. In 2016, just the digit position 16 will be lit.

The colours of the LEDs have no significance – they are different to make the display more interesting. If you’re using the Sense HAT, you may need to install the software for the HAT using the following commands:

![](img/d8322379620245fcd21eccc3855b8c78_137_1.png)

```
sudo apt-get update
sudo apt-get install sense-hat
sudo pip3 install pillow
```

If you’re using a Unicorn HAT, then run the following commands:

```
sudo apt-get install python-pip python-dev
sudo pip install unicornhat
```

There are two versions of the software for this project – one for the Sense HAT and one for the Pimoroni Unicorn HAT – so be sure to use the right version of the program for your hardware. The Sense

![](img/d8322379620245fcd21eccc3855b8c78_138_0.png)

USB WiFi adaptor
for internet time

Sense or Unicorn HAT

HAT version is called binary_clock_sh.py, and the Unicorn version binary_clock_uni.py.
You can download the programs for this project from your Raspberry Pi command line using the command:

```
git clone https://github.com/simonmonk/pi_magazine.git
```

This command will actually bring down the code for all the projects in Simon’s The MagPi series, so if you’ve already issued this command for one of the earlier articles, change directory to pi_magazine and run the following command to update your directory

with this project (12_binary_clock):

```
git pull
```

To run the program, change to the directory where the code for this project lives and then run the program using the commands below:

```
cd /home/pi/pi_magazine/12_binary_clock
sudo python binary_clock_sh.py
```

Or, if you are using the Unicorn HAT:

```
sudo python binary_clock_uni.py
```

### binary_clock_uni.py

```
#!/usr/bin/env python

import unicornhat as hat
import time, datetime

year_color = (0, 255, 0)
month_color = (0, 0, 255)
day_color = (255, 0, 0)
hour_color = (0, 255, 0)
minute_color = (0, 0, 255)
second_color = (255, 0, 0)
hundredths_color = (127, 127, 0)
off = (0, 0, 0)

hat.clear()
hat.brightness(0.5)

def display_binary(value, row, color):
    binary_str = "{:08b}".format(value)
    for x in range(0, 8):
        if binary_str[x] == '1':
            hat.set_pixel(x, row, color[0], color[1], color[2])
        else:
            hat.set_pixel(x, row, 0, 0, 0)

while True:
    t = datetime.datetime.now()
    display_binary(t.year % 100, 0, year_color)
    display_binary(t.month, 1, month_color)
    display_binary(t.day, 2, day_color)
    display_binary(t.hour, 3, hour_color)
    display_binary(t.minute, 4, minute_color)
    display_binary(t.second, 5, second_color)
    display_binary(t.microsecond / 10000, 6, hundredths_color)
    hat.show()
    time.sleep(0.0001)
```

64 32 16 8 4 2 1

- Year
- Month
- Day
- Hour
- Minute
- Second
- 1/100 of a Second

![](img/d8322379620245fcd21eccc3855b8c78_139_0.png)

### binary_clock_sh.py

```
#!/usr/bin/env python

from sense_hat import SenseHat
import time, datetime

hat = SenseHat()

year_color = (0, 255, 0)
month_color = (0, 0, 255)
day_color = (255, 0, 0)
hour_color = (0, 255, 0)
minute_color = (0, 0, 255)
second_color = (255, 0, 0)
hundredths_color = (127, 127, 0)
off = (0, 0, 0)

hat.clear()
```

```
def display_binary(value, row, color):
    binary_str = "{0:08b}".format(value)
    for x in range(0, 8):
        if binary_str[x] == '1':
            hat.set_pixel(x, row, color)
        else:
            hat.set_pixel(x, row, off)

while True:
    t = datetime.datetime.now()
    display_binary(t.year % 100, 0, year_color)
    display_binary(t.month, 1, month_color)
    display_binary(t.day, 2, day_color)
    display_binary(t.hour, 3, hour_color)
    display_binary(t.minute, 4, minute_color)
    display_binary(t.second, 5, second_color)
    display_binary(t.microsecond / 10000, 6, hundredths_color)
    time.sleep(0.0001)
```

Language
>PYTHON
DOWNLOAD:
magpi.cc/1P1182l

### How the code works

The two versions are actually very similar to each other. Both start by importing the libraries that they need for their displays. They then define a number of colour variables that you can change, if you like, in order to set different colours for each row of the clock.

Most of the action takes place in the display_binary function. This takes three parameters. The first is the value to be displayed as binary, the second is the row on which it should be displayed (0 to 7), and the final parameter is the colour to be displayed.

The value is converted into a binary string representation using String’s format function. The for loop then iterates over each column (x value) and if that digit contains a 1, turns on the pixel in that row and column to the colour specified, otherwise the pixel is cleared.

The main loop fetches the current time and then splits it into its component parts, displaying each on a separate row. To display the year in two-digit format, we use modulo 100 (t.year % 100), which is the remainder after dividing it by 100.

### Using your binary clock

Having a keyboard, mouse and monitor attached to your clock is fine while you are constructing it, but it would be better to have the program start

> The two versions are actually very similar to each other.

automatically when the Raspberry Pi first boots up. To do this, run the following command to make the program executable:

```
sudo chmod +x binary_clock_*.py
```

Then, edit the file /etc/rc.local with the command:

```
sudo nano /etc/rc.local
```

Add the following line after the first block of comment lines that begin with #. Change the line to the _uni version if you are using a Unicorn HAT.

```
sudo /home/pi/pi_magazine/12_binary_clock/binary_clock_sh.py &
```

Restart your Raspberry Pi and this time the program should start up automatically.

Note that you will still need an internet connection for the Pi to pick up the correct time.

## RASPBERRY PI ZERO

A new revolution in computing is upon us

We’d like you to pick up the Raspberry Pi Zero that came with The MagPi 40. You’re holding in your hands a fully functional computer that’s going on sale for $5 and that you could lose in a small purse. It’s quite the amazing feat of engineering and we’re very excited to bring it to our readers, along with many pages of content that will take you from a Zero Novice to a Zero Master.

Over the course of our expansive coverage, we talk to the people behind the Raspberry Pi Zero’s development, teach you how to hook it up, and also suggest some great uses for your Zero. With its even smaller size, there are some amazing things you’ll be able to do with it. Check out the contents at the start of this section to get an idea of what kind of incredible things we’re about to do with the Pi Zero.

MicroSD card slot for storing the operating system, files, and documents

30mm

Mini-HDMI port for digital sound and 1080p video

![](img/d8322379620245fcd21eccc3855b8c78_141_0.png)
