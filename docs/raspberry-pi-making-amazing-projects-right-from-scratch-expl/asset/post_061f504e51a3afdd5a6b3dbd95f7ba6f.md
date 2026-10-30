
| Port | I/O Pin | Label/Purpose |
|------|---------|----------------|
|      | 5 (GP5) | Arm/Disarm Buzzer (Output) |
|      | 6 (GP6) | Alarm LED (Output) |
|      | 7 (GP7) | Alarm Bell (Output) |

## Arming yourself

The terms arm and disarm are alarm system-speak for switching the alarm monitoring on (arming the system) and off (disarming the system). Zone 1 of our system is going to be linked to the arming and disarming part of the system as it will be connected to the sensors on the door that we leave or enter from; this will be a special zone for entry or exit purposes.

When we set the alarm, we need a bit of time to get out of the house. The way that the system knows we've left the property is by monitoring the exit zone to see if we've opened and then closed the front door behind us within the time allowed.

Similarly, when we return, we will open the front door, but we don't want the alarm to go off straightaway — we need a chance to disarm the system within a given amount of time. We will arm and disarm the system via our web-based control panel, or by using a switch of some sort on the input GP0.

## The master configuration file

Our system will use a master configuration file that will tell it how everything is set up and connected. This configuration file will be used by both the web control panel and the main alarm control scripts so that the two sub-systems can "talk" to each other. Let's create the file with our initial settings.

The settings file will be stored in the same location as where we will create our control scripts in Chapter 9, Putting It All Together, which is in the folder /etc/pi-alarm. So, let's create this folder, and give it execute rights so that our scripts can be run:

```text
$ cd /etc
$ sudo mkdir pi-alarm
$ sudo chmod 777 pi-alarm
```

We'll now create the master configuration file, to be used by our system, in this folder:

```text
$ cd pi-alarm
$ sudo nano alarm.cfg
```

As before, you don't have to create your files in Nano on the Raspberry Pi—you can create them on your desktop computer, and then transfer them to your Pi using SCP.

```text
# ALARM MASTER CONFIG FILE #

#Number of zones in the system
NUM_ZONES=8

#Display labels for each zone
ZONE_LABEL_1="Zone 1 - Entry/Exit"
ZONE_LABEL_2="Zone 2"
ZONE_LABEL_3="Zone 3"
ZONE_LABEL_4="Zone 4"
ZONE_LABEL_5="Zone 5"
ZONE_LABEL_6="Zone 6"
ZONE_LABEL_7="Zone 7"
ZONE_LABEL_8="Zone 8"

#Zones that are enabled
#Set to 0 to Disable or 1 to Enable
ZONE_ENABLE_1=1
ZONE_ENABLE_2=1
ZONE_ENABLE_3=1
ZONE_ENABLE_4=1
ZONE_ENABLE_5=1
ZONE_ENABLE_6=1
ZONE_ENABLE_7=1
ZONE_ENABLE_8=1

SYSTEM_ARMED=0

#Zone status
#Set to 1 if zone is triggered
ZONE_STATUS_1=0
ZONE_STATUS_2=0
ZONE_STATUS_3=0
ZONE_STATUS_4=0
ZONE_STATUS_5=0
ZONE_STATUS_6=0
ZONE_STATUS_7=0
ZONE_STATUS_8=0
```

alarm.cfg file

## Creating the web page

Our Web-based control panel is going to be a single PHP-driven HTML5 web page which will be **mobile optimized**. HTML5 is the latest mark-up standard for web pages and is supported by most modern smartphones and browsers. We will also create a **cascading style-sheet (CSS)** that will make our page look half reasonable on mobile devices.

To create the web files, I recommend that you use something like the excellent Notepad++ on your desktop computer, rather than doing it directly on the Raspberry Pi. Alternatively, if you are a seasoned web developer, you may already have your IDE of choice.

### The control panel HTML template

The first thing we'll do is create an HTML file that we can use to test our layout before we put the HTML into a PHP file to make it interact with our system. This makes it easier to tweak the way we want it to look beforehand, without the PHP scripts getting in the way.

> This is not a tutorial on Web development—there is a plethora of books out there on that subject—but I hope the code is clear enough for you to work out what's going on. The code I'm going to show you is fully functional, so you can just use what I give you without having to do any more. Hopefully, it makes your control panel look OK too!

The following mark-up gives you a basic control panel with status for our 8 zones, a master arm and disarm switch, and switches to enable or disable any of our zones.

The `<head>` section of the code contains some `<meta>` tags that help mobile devices know that it's a mobile-friendly site. In the main `<body>` mark-up, we have a section for each zone that contains the zone's name and an on/off switch. Each zone is in its container so that we can also highlight a particular zone that needs our attention, for example, if it's triggered.

You can find the full HTML5 markup for our control panel in the `alarm-panel.html` file located inside the code folder of `chapter 7`.

### Giving it some style

At the moment, this page doesn't look that great (in fact, it looks awful, like something from the 1990s); it isn't particularly good for mobile devices and would most certainly fail the *sausage test*. So, we're going to apply some styling to make it look not half bad. Although the preceding mark-up contains a reference to a CSS file—we haven't created that file—so this is what our page currently looks like (as I said: it looks awful):

![](img/631e8b200cce187082ee46b0eb1681b3_397_0.png)

The web control panel without any styling

The following CSS3 mark-up is designed specifically for our control panel, and it makes it look quite nice while also making it usable on **touch-screen** mobile devices. The CSS is quite long and seems overwhelming, but you don't need to do anything with it, or understand it, if you don't want to—the only thing you need to know is that it's been designed for modern browsers and smartphones, so don't expect it to work on Internet Explorer 7, or probably even IE9!

In essence, it contains the styling for the following:

- Preparing the browser for our mobile layout
- Our text and zone areas
- Creating cool switches instead of boring checkboxes
- Making an area flash on and off when we need it to

```css
/* Clear browser margin and padding defaults */
body, div, dl, dt, dd, ul, ol, li, h1, h2, h3, h4, h5, h6, pre, form,
fieldset, input, textarea, p {
margin:0;padding:0;-webkit-text-size-adjust:none;
}

body {
    background: #ffffff;
    color: #4A5651;
    font-family: "Trebuchet MS", Helvetica, sans-serif;
    font-size:10px;
    height: 100%;
    padding:0;
    margin:0 auto;
    max-width:320px;
    min-width:240px;
    text-align: left;
    width:100%;
    -webkit-box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
    -moz-box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
    box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
}

p, .zoneLabel {
    font-size:16px;
    margin:5px;
    line-height:1.4;
    color:#4A5651;
}

#header h1 {
    font-size:20px;
    line-height:40px;
    margin:0;
    padding:0 0 0 15px;
    text-align:center;
    text-overflow: ellipsis;
    font-weight:bold;
}

.zoneControl, .masterControl{
  border-bottom:1px solid #dddddd;
  margin-top:5px;
  margin-bottom:0px;
  padding:5px;
  display:block;
  width:100%;
}

.zoneLabel {
  font-weight:bold;
  text-overflow:ellipsis;
}

input[type="submit"] {
  border: none;
  background-color: #0b70cc;
  color: white;
  height: 32px;
  display: block;
  padding: 4px 7px;
  float: left;
  border-radius: 8px;
  position: relative;
  bottom: 1px;
  margin-left: 4px;
  text-align: center;
}
input[type="submit"]:hover {background-color: #b2ceec;color: #0b70cc;border: none;border: 1px solid #b2ceec;}

/* Flashing animation */
@-webkit-keyframes flash{0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
@keyframes flash {0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
.flash {
  -webkit-animation-name:
  flash;animation-name:
  flash;color:#f00000;
}

.animated {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
  animation-iteration-count: infinite;
  -webkit-animation-iteration-count:infinite;
}

/*
  ON/OFF SWITCH STYLES
  The rather cool On/Off switch styling was generated on
  https://proto.io/freebies/onoff/
*/
.onoffswitch {
  position: relative;
  width: 90px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}

.onoffswitch-checkbox {
  display: none;
}

.onoffswitch-label {
  display: block;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid #FFFFFF;
  border-radius: 20px;
}

.onoffswitch-inner {
  display: block;
  width: 200%;
  margin-left: -100%;
  transition: margin 0.3s ease-in 0s;
}

.onoffswitch-inner:before, .onoffswitch-inner:after {
  display: block;
  float: left;
}
```
