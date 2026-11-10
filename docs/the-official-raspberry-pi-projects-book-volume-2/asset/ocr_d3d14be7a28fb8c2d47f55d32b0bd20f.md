(command line interface) and enable ‘Wait for network’. You can also set these options with the command line utility raspi-config. Make sure you’ve also configured your Pi to connect to your WiFi network. Alternatively, you can use a USB Ethernet adaptor with your Pi Zero.

# Software setup

From the command line, download the Python Slack Client library:

```
sudo pip install
slackclient
```

Download the project code:

```
git clone https://
github.com/mrichardson23/
slacktivity-monitor.git
```

You’ll need to use your own Slack API token in the project code so that the Slacktivity Monitor can authenticate itself. While logged into Slack, go to magpi.cc/1Vai8OF and click Create Token for the user and team that you want to monitor. Copy that token code into your code, replacing the text SLACK TOKEN HERE.

The code keeps track of roughly the last minute of activity. If there have been no messages sent within that time period, then the red LED will illuminate. If there have been any messages (but less than 15) within the last minute, then the yellow LED will illuminate. If 15 or more messages have been sent within the last minute, the LED will turn green. You may need to adjust this threshold to match the typical levels of activity in your Slack team.

Set up the script to run at boot. From the command line, execute:

```
sudo nano /etc/rc.local
```

Before the exit 0 line, add a line to boot your code:

```
python /home/pi/
slacktivity-monitor/main.
py &
```

![](bbox=[0.305, 0.108, 0.535, 0.312])

In order to connect the Pi Traffic Light to a Raspberry Pi Zero, you’ll need to solder header pins.

When you’re done, save the file and shut down the Pi so that you can solder the headers, add the Pi Traffic Light, and attach it where you need it.

# Physical build

Solder header pins onto the Pi Zero and connect the Pi Traffic Light to those pins. The code uses pin 15 for red, 14 for yellow, and 28 for green. This is because they’re all next to each other along with a ground pin, so the Pi Traffic Light will plug right into the header pins and work like a charm. Depending on how you want to arrange your Slacktivity Monitor, you may want to change the pin numbers and colours.

Use Blu-Tack or Command Strips to attach the Slacktivity Monitor to your display.

When you power up the Pi, the script should start automatically and connect to your team’s instance of Slack. Even if there’s no recent activity, you may see the traffic light turn yellow initially because the Slack API will return the most recent message when a new client connects.

# Going further

The code ignores everything except messages, so you could also adjust the Slacktivity Monitor to react to different types of events. For instance, you can use the red light to indicate when your boss is offline, yellow when she’s idle, and green when she’s active.

# Main.py

```
import time
from slackclient import
SlackClient
from gpiozero import LED

token = "SLACK TOKEN HERE" # found at
# https://api.slack.com/web#authentication

red = LED(15)
yellow = LED(14)
green = LED(18)

red.off()
yellow.off()
green.off()

sc = SlackClient(token)

message_counts =[]

if sc.rtm_connect():
    while True:
        message_count = 0
        events = sc.rtm_read()
        for event in events:
            if event['type'] == 'message':
                message_count = message_count + 1
                message_counts.insert(0, message_count)

            if len(message_counts) > 600:
                message_counts.pop(600)
                total_message_count = 0

            for i in range(0,len(message_counts)):
                total_message_count = total_message_count + message_counts[i]

            if total_message_count == 0:
                red.on()
                yellow.off()
                green.off()

            if total_message_count > 0 and total_message_count < 15:
                red.off()
                yellow.on()
                green.off()

            if total_message_count >= 15:
                red.off()
                yellow.off()
                green.on()

            time.sleep(.1)
else:
    print "Error. Invalid token?"
```