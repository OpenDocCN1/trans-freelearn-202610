# PLAYING IT BY EAR WITH PIANO HAT

The latest HAT from Pimoroni is a great way to unleash your ivory-tinkling tendencies. Let’s use it to build a relative pitch tester

+   You'll Need
+   - A Piano HAT
+     magpi.cc/1OALwNT
+   - The Piano-HAT library
+     magpi.cc/2dXkyIM
+   - Headphones or an external speaker

Relative pitch is the ability to identify a given musical note by comparing it to a reference note. Unlike perfect pitch, relative pitch can be improved with training. The Piano HAT is a versatile piece of hardware that we can use to create a fun game that tests people’s skill in recognising different notes. It was inspired by Zachary Igielman’s legendary PiPiano and it turns your Pi into a functional musical keyboard. Each of the 16 capacitive keys also has an LED so you can create you own ‘learn to play’ tutorials or just give your performances a visual appeal.

# >STEP-01
Getting started with Piano HAT

Like most HATs, this one is straightforward to use. Simply plug it carefully onto the GPIO pins of your Pi. Then install the Piano-HAT Python library. This requires the I²C bus on the Pi to be enabled, and there are plenty of instructions for this online. But to make life super-easy, those Pirates at Pimoroni provide a handy script that takes care of everything:

```
curl -sSL get.pimoroni.com/pianohat | bash
```

# >STEP-02
Wired for sound

There are two options for getting audio output from a Pi. If you are using a HDMI monitor or a TV that has built-in speakers, the audio can be played over the HDMI cable. If not, you can switch to use headphones or a speaker plugged into the headphone jack. The Pi will normally auto-detect the available outputs, but sometimes it gets this wrong. To force audio to use a specific output, you can use this command:

```
amixer cset numid=3 2
```

The second number determines the output: HDMI = 2, jack = 1, auto = 0.

# >STEP-03
Play it again

The Piano-HAT library has a nice collection of demonstration Python scripts. A good one to start with lets you use the Piano HAT as… a piano!

```
sudo python Pimoroni/pianohat/simple-piano.py
```

![](bbox=[0.02, 0.6, 0.65, 0.94])

Each touch panel has a corresponding LED

Sixteen capacitive touch panels are used as the keys and control buttons