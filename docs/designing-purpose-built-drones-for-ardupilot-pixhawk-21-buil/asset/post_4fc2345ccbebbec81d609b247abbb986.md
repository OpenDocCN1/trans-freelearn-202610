
## Summary

In a book about Pixhawk drones, you may be asking, "Where's the programming of the Pixhawk?!" We'd be remiss in a book about designing purpose-built drones if we didn't cover something on the water. Plus, we want to show you that there are similarities between land and water drones. What we really wanted to get deeper into is the *design and prototyping* of an idea. Now that we've covered the modeling, and construction of your own design (we covered modeling, printing, water-sealing, design, sketching, identifying problems, and construction testing); in the next chapter, we're going to get much deeper into the Pixhawk.

We'll be taking a (pretty much) fully constructed vehicle (a motorized golf trolly), and adapting it for Pixhawk. We'll also be getting into using a mobile device (via Bluetooth) to control your drone, as well as getting deeper into the configuration of Pixhawk.

From here in, now that you're an expert in turning your construction designs into reality we're going to be focusing more on the Pixhawk device itself.

So, buckle up!

## 4

## A Drone for Golfers

As we've stated a few times before, when choosing an industry to make a drone for, you have to balance the size of the market (demand), with how much money they have to spend (cost), and how easy it will be for you to fill that demand (supply).

One of the industries that is extremely popular is golf. Every year, consumers literally spend billions of dollars on golf equipment and accessories. One thing golfers go nuts about is the new technology to help their game. Sounds pretty much perfect, right?

So, where can a drone fit in? A multicopter to fly down the fairway and check the layout? Maybe, but if the golfer has played that course before, why would they need it? They wouldn't. Ok, that's a bad idea. What about a submarine drone to look for a golf ball that went into the water hazard? Not a terrible idea, but there are probably so many balls sitting at the bottom of a water hazard that finding your own ball may prove impossibly difficult. And who wants to keep pulling out golf balls that have been damaged by weeks of sitting at the bottom of a pond? Nope, another bad idea.

What's something that every professional golfer has, and every amateur wants (but may not be able to afford)? A caddy (someone to carry your clubs around the course)! So, what if we took a motorized golf trolly (a small cart that carries golf clubs) and made it follow a cell phone?

Now, we're on the right track.

We'll be covering the following topics:

- The design
- Using tank locomotion
- Using LIDAR to avoid obstacles

## The design

After some initial research, we found that there is already such a device out on the market. It's called the Stewart X9 Follow. However, at a list price, this item is just shy of $3,000 US!

![](img/504adb30bdfce87f38258e08f11180af_112_0.png)

It's certainly a high-quality product—a very sleek and beautiful design. There is no doubt that the high-end golfer would love to indulge in this item. But for your average golfer, this would absolutely break the bank.

This item also uses its own remote. The rover follows the remote, or can be piloted manually with it. We're going to use a different approach. Instead of a proprietary remote, we'll make ours follow a smart phone (via Bluetooth or Wi-Fi). This means it can be paired easily. Also, we'll include a standardized RC remote (in the event that the user's phone runs out of battery because they forgot to charge it).

Using standardized parts should allow us to bring the final cost of our unit down to around $1,000 US. Our unit may not end up as luxurious as the Stewart, but it will hit a much wider section of the market.

Here's our original sketch:

![](img/504adb30bdfce87f38258e08f11180af_113_0.png)

Fabricating an entire golf caddy could get quite expensive. So, let's start off with someone else's (already motorized) caddy. We found the following on Amazon:

![](img/504adb30bdfce87f38258e08f11180af_113_1.png)

The beauty of this trolley is that it already has dual motors (as it's built to be piloted via remote). All we really have to do is implement a brain in this dumb caddy. Then, it can intelligently follow a cell phone.

We are going to have to do a complete rewire to make this work though. The motors are brushed, so we're going to need two brushed ESCs. These speed controllers also need to be capable of reverse as well. Why? Because to turn the trolley, we will use a *tank* like steering (where one side goes forward and the other side reverses).

Now, we need to lay out exactly how everything will connect, so let's look at our wiring diagram:

![](img/504adb30bdfce87f38258e08f11180af_114_0.png)

You may notice that we didn't include a **Battery Elimination Circuit (BEC)** to provide power to the servo section on the Pixhawk. That's because we're going to use ESCs that have BECs built in. This will provide voltage to the servo board via the ESC servo connections.

You'll also notice that at this juncture we hadn't decided whether to use Wi-Fi or Bluetooth as our communication protocol. We've eliminated the 900 Mhz data connection for laptops in favor of one of these protocols. Why? Because nobody's going to want to carry around a laptop on the golf course. Instead, we're going with a mobile device.

So, what's the difference? If we connect via Wi-Fi, the Pixhawk will become a network access point. This may actually present a problem because GPS requires a data connection on your phone to display maps. If we use Wi-Fi, the network is established with Pixhawk from your mobile device (rather than your service provider's cellular data connection).

So how do we make Pixhawk use a tank style drive system to steer the vehicle? Simple, really.

## Adapting Pixhawk for skid-steering

The first thing is to plug the right ESC into the right port on the servo panel of the Pixhawk. The left wheel's ESC plugs into MAIN OUTPUT 1. The right wheel's ESC should go into MAIN OUTPUT 3 as shown in the following diagram:

![](img/504adb30bdfce87f38258e08f11180af_116_0.png)

Now, we just have to enable skid-steering in the Pixhawk itself. It'll do all the work of figuring out what to do to what wheel all on its own!

Inside of the Mission Planner software, connect to the Pixhawk (after plugging in the USB cable of course). Enter the Config/Tuning screen and select Standard Params as shown in the following screenshot:

## Using Bluetooth to control Pixhawk with a phone

Bluetooth with Pixhawk is a snap. But just like every other bit of Pixhawk functionality, an add-on module needs to be purchased. We picked up the one shown as follows:

![](img/504adb30bdfce87f38258e08f11180af_118_0.png)

Of course, we had to alter the cable (just as we did before) because the cable included with this module is for the old-style plugs. We connected our Bluetooth module to the Telem 1 port as shown in the following figure:

![](img/504adb30bdfce87f38258e08f11180af_119_0.png)

Now, it's just a matter of binding our Android device's Bluetooth to the Pixhawk's Bluetooth. Just power on your Pixhawk and bind using the same method you would for any Bluetooth device. The software to control Pixhawk from a mobile device is called DroidPlanner 2. It's free!

We're not quite there yet, but if you're curious on how to get a Pixhawk vehicle to follow you using DroidPlanner 2, it's easy! Just click on the **Follow** button, and set the radius you'd like the rover to stay within. Of course, you can ignore the `altitude` attribute (as this is a ground vehicle). You can see the **Follow** button in the following picture:

![](img/504adb30bdfce87f38258e08f11180af_120_0.png)

Of course, if you don't want the rover to follow you onto the putting green, just tap **Pause**. You can also click on the points on the map to send the rover off!

Pretty cool, huh? But what if there's a tree between you and your rover? You don't want it hitting trees and boulders while it's trying to get to you, right? Enter collision avoidance!

## Using LIDAR to avoid obstacles

Would you like to go for a walk on a golf course blindfolded? No? Me neither, and it's probably not good to just set a rover to run around blind either. But vision and object recognition takes a lot more computing power than we're budgeted for on this machine. So, what's the next best thing to vision? SONAR or LIDAR. Unfortunately, SONAR (just like a bat finds insects using sound waves to calculate the distance to obstacles) is considered *passé* for Pixhawk, and thus the implementation is quite *wonky*. There are I2C sonar devices, but they don't work very well (if at all) with Pixhawk 2.1 and the latest Ardupilot firmware. So, we're going to use LIDAR instead. LIDAR rangefinders use light waves (instead of sound) to calculate the distance to objects.
