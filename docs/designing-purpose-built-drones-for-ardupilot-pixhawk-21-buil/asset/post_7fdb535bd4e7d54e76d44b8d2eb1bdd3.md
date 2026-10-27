
A two-position switch would be a PWM < 1230 (off) and a PWM >1750 (on). With more positions on the switch, the number would fall somewhere in between. If the remote is paired and powered on while you’re on this screen, as you flip through the positions of the switch, the drop-down menu will highlight green. Just set the desired mode for each position. Make sure you click on Complete before clicking on Next.

We’ve been through calibration before. We’re only showing this screen at this point to emphasize that you should not forget to reverse any channels that are moving in the wrong direction on your transmitter:

![](img/504adb30bdfce87f38258e08f11180af_173_0.png)

Next, we need to tell Pixhawk how to handle emergencies.

![](img/504adb30bdfce87f38258e08f11180af_174_0.png)

Failsafes are engaged during (what most would call) emergency situations. They can range from engaging a different autonomous flight mode to preventing a user error. Let's take a look at these:

- **Battery Failsafe Enable:** This sets the flight mode to engage if the battery's capacity drops below safe levels.
- **Ground Station Failsafe:** This sets the flight mode if the connection is lost with the ground station. As we're using a ground station (heavily), and don't plan on long-range autonomous flight, we'll set this mode to RTL.
- **Throttle Failsafe Enable:** This sets whether or not the pilot can drop below a specified (using the slider) level during flight. We would neither want to turn off our motors and tumble out of control, nor fly through our own propwash during descent (potentially resulting in a crash).

Geo fencing is another feature you can enable in the setup wizard:

![](img/504adb30bdfce87f38258e08f11180af_175_0.png)

Geo fencing allows you to limit the range and altitude of your drone. The actual limits are pretty self-explanatory, so let's move on.

## Configuring the ground station

Here is where we configure our ground station to receive video, use joysticks, and just be all around awesome! Let's start with the joysticks.

As you can see there are lots of buttons. There's even a Thinkpad style button mouse that you could potentially use to control a gimbal (if we were interested in doing that).

In Mission Planner, we'll go into the **Config/Tuning** section and select the **Planner** button from the left side. We'll see the following screen:

![](img/504adb30bdfce87f38258e08f11180af_178_0.png)

Here is where we can set what every button, stick, dial, and slider on our physical joystick will control.

Once every axis is set on our main controls for **Roll, Pitch, Throttle,** and **Rudder**, you'll need to make sure that they control the green bars in the correct direction. If they don't, just tick the checkbox in the **Reverse** column.

Setting an axis or button is relatively easy. Just click on the **Auto Detect** button, and an alert will pop up telling you to click on **OK**. Don't move the joystick until you do. Once you click on **OK**, the software will begin listening for movement on the game controls. As soon as you move an axis or press a button, it will register that as the desired control.

You may notice that several buttons have a -1 selected in their **Control Axis** dropdowns. This is a designation which means that the button is unassigned. As with the axis, the **Auto Detect** buttons can automatically assign the physical joystick button to that control.

The next dropdown is the type of control that button is assigned to. You may notice that several are set to **Change Mode**. That's because they set the drone into various flight modes. You can set the flight mode using the **Settings** button next to the dropdown. It will open another window, with yet another dropdown in it.

Similarly, you'll find the `Do_Set_Servo` type very useful. It can do all sorts of things, from setting servo-plug controlled gimbal modes, to controlling camera functions via a servo-plug, to raising and lowering retractable landing gear. Basically, it can set a servo value for just about anything controlled via servo-plug. All you have to do is open the **Settings** dialog, and select the output port on Pixhawk, then tell it what you want that PWM value of the control to be (remember, the PWM value we were talking about when we set up flight modes on the controller). You're just mimicking what you'd like it to be as if you flipped a switch on a standard RC remote. On our two `Do_Set_Servo` functions, we're using a toggle switch on the joystick. One direction sets the PWM to 0 (lowered landing gear), and the other direction sets the PWM to 1,800 (raised landing gear).

Once you've finished configuring a joystick, all you really have to do is click on **Save** at the top of the interface, and then click on **Enable** before closing the window.

If everything is working properly, you'll see the following in the upper-left of the map window:

![](img/504adb30bdfce87f38258e08f11180af_181_0.png)

## Setting up video

There are essentially two components to getting the video up on the laptop screen:

1. Receive the transmission.
2. Digitize it into the laptop.

We chose our Spektrum FPV camera and transmitter because it was inexpensive and light. But we also chose it because it uses the most common set of frequencies in FPV: Fatshark. So, finding a Fatshark receiver for our ground station will be easy and also inexpensive. Here's the one we arrived at:

![](img/504adb30bdfce87f38258e08f11180af_181_1.png)

What this does is it swaps out the artificial horizon on the upper left of our flight interface with the FPV view and **on-screen display (OSD)** telemetry overlaid on it as shown in the following screenshot:

![](img/504adb30bdfce87f38258e08f11180af_183_0.png)

And there we have it! We're ready to do our test flights, right? Wrong. If you remember, during the assembly stage we needed to put off part of that until we could spin up our motors. We're at that stage. If you notice from the FPV camera there are no propellers on the drone. Never even think about attempting the next stage unless you've removed the propeller first.

## Final configuration before test flights

Before trying to fly this thing, we need to make sure that every motor is hooked up to the right spot and is turning in the right direction. Remember our chart from before? Here's just the Hex-X from it as well as the Pixhawk with the six-motor servo ports highlighted:

![](img/504adb30bdfce87f38258e08f11180af_184_0.png)

## Summary

In this chapter, we learned a bit more on the philosophy of prototyping, kitbashing, and altering prefab parts for our purposes. We also learned how to configure Pixhawk for a multicopter, include a video feed in our laptop display, and use joysticks to control a drone. If that wasn't enough, we also learned how to balance power to weight on a multicopter. Now, you can see just what complicated beasts they are.

In the next chapter, we're going to take on the Holy Grail of dronery: a fixed wing drone. In a lot of ways, they're simpler than multicopters. But in reality they're not.

## 7 The Holy Grail - A Fixed Wing Drone

Wow for a holy grail, she sure is one ugly pig, right? Well, prototypes just aren't usually pretty. So, why is a fixed wing drone (like the airplane in the following image) considered a holy grail?

![](img/504adb30bdfce87f38258e08f11180af_188_0.png)

The answer is pretty simple, actually. Airplanes need to keep moving in order to stay airborne. Go too slow, and you stall (a fancy word for fall from the sky until you build up speed again except that it's usually vertical speed, and vertical speed is going down).

With multicopters, tuning them is fairly easy: take it off, hover just **outside of ground effect** (OGE), and let it *twitch* around a bit. If it's too crazy, just land it, reset the settings, and repeat. Since an airplane is constantly moving tuning can be quite a bit more hazardous to the aircraft.
