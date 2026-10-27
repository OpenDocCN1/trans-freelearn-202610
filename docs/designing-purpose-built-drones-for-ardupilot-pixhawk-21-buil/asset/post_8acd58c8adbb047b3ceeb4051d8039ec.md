
![](img/504adb30bdfce87f38258e08f11180af_135_0.png)

Here, you can see that each spar is 45° from each other, and each rotor is 2'9" from the center of the airframe to the center of the rotor. Add another 22", and you have a full blade-span of 4'7". Yes, it's a biggie. You may also notice that each spar is shorter than the exact center of each rotor. This is to allow for a motor-mount that is designed to fit in, using a sleeve, over the spar.

The math on airframes is easy. Just make the spars long enough so the blades have some clearance (and don't cause a ton of turbulence for each other), set the motors at symmetrical points around the center, and use even numbers of rotors. Personally, I don't advise tricopters. They are inherently unstable because they have an odd number of rotors.

There's a lot more that goes in if you want to get fancy (changing the angle of the rotors to assist with turning, thrust, and so on). But these are the basics.

## Designing a fixed wing - airplane

Buckle up, because this is rocket science. Airplanes need to keep moving forward to stay in the air. We want as little wind resistance (drag) as possible when designing an airplane. This will let it stay in the air longer and relieve some stress on our motor.

There are hundreds of design motifs for airplanes, but let's use the three major ones (as shown in the following diagram):

![](img/504adb30bdfce87f38258e08f11180af_136_0.png)

If the flying wing style is confusing, take a quick look back at the picture of the Skywalker X-8. That is a flying wing. Here's an example of a jet-propelled drone I'm currently designing, which is also a flying wing. It uses Electric Ducted Fan (EDF) jets rather than propellers to keep it going:

![](img/504adb30bdfce87f38258e08f11180af_136_1.png)

Obviously, this one is meant to go fast, very fast. But using EDF jets has a tradeoff—very short flight times and very high battery consumption. So, we'll stick with propellers.

Even with this tool in place, it's still a bit confusing, right? Let's talk about these parameters and what they mean:

- **Chord (mm)**: Simply put, it's the entire length of your airfoil. It's the straight line from the front tip to the back tip.
- **Radius (mm)**: This is the radius of the curve used to generate the very front curve on the airfoil (the leading edge).
- **Thickness (%)**: This is the vertical measurement at the thickest part of the wing.
- **Origin (%)**: This has zero impact on the airfoil shape. It just moves the origin point of the grid backward from the nose of the wing.
- **Pitch (degrees)**: This is how much the overall design of the wing is pitched up from being level.
- **Halo (mm)**: This has zero impact on the airfoil shape. It just draws a line inside or outside of the outer line to help if you're using this image to cut with a jig.
- **All other parameters**: This is also just for how the airfoil is drawn for your plans from grid size to paper size.

See? It's not really that bad. It sure is a heck of a lot harder than a multicopter though, and the development cycle is much, much longer. You'll need to test fly the airplane several times and tweak the design until you're happy (long before implementing Pixhawk).

## Summary

In this chapter, you got a good primer for all that is involved in designing an aircraft and the differences between an aircraft and a rover. I'm sure now you can see why we started with rovers. Compared to the aircraft, they're a piece of cake.

In the next chapter, we'll be showing you how to implement Pixhawk into a multicopter. We won't be focusing on designing a multirotor so much as making it work. So, we'll kitbash a DJI Flamewheel F550 airframe. Enjoy!

## A Simple Multicopter Drone

Multicopters are very complex beasts. An awful lot can be done with Pixhawk and multicopters. Through the programming interface **Micro Air Vehicle Link (MAVLink)**, all sorts of missions can be triggered automatically. From an alarm system trigger sending a drone out to a specific waypoint (based on the sensor triggered), to missions that adjust to varying conditions such as weather, light conditions, or any other number of variables only limited by your imagination—multicopters are the most versatile, yet complex, systems available for your design and concepts.

For the purposes of this book, we're going to keep things simple though. Once you're ready to take the leap into programming full applications for the MAVLink interface, there are great tutorials and a full coding guide at the *QGroundControl* website (`http://www.mavlink.org/dev/mavlink_groundcontrol_integration_tutorial`).

What we're going to do is show you how to design one of the most complex machines that Pixhawk can control—a multicopter drone. Specifically, we're going to show how to design and prototype a 360 VR video multicopter.

![](img/504adb30bdfce87f38258e08f11180af_142_0.png)

Of course, our proof of concept gimbal won't be anything near this elaborate but the principle of their operation is the same. We need something to dampen vibration, and counter the constant tilting and rolling of a multicopter.

But we're getting a bit ahead of ourselves. What's a nice, inexpensive camera we can use to do some 360 VR video on a proof of concept?

### Starting with the payload

The drone is just a delivery device for its payload, in this case, a camera. So, it makes sense when designing any drone to start with what it is that you're trying to carry. On any proof of concept, the idea is to spend as little money as possible to prove your concept. Just start with the assumption that you will encounter problems and maybe even crashes (especially with a multicopter). Why risk a $10,000 camera (or even a $1,000 camera) on a proof of concept? Start small, and then scale up!

So, we found the perfect camera—a Ricoh Theta S. The Theta S is small, lightweight, has an integrated (built-in) battery, and an integrated memory card. The less we have to worry about, the better, right? Best of all, it shoots in the same 360 format as any high-end 360 camera, and only costs $326! It's quite simply the GoPro of 360 Cameras. The Amazon listing for the Theta S is shown in the following screenshot:

![](img/504adb30bdfce87f38258e08f11180af_143_0.png)

Also (as you can see on the listing), we already owned one! Always try to use things already in your shop for proof of concept. It saves a lot of cash! We purchased ours the previous summer to do some prototyping of our 360 VR editing workflow in postproduction.

The problem we'll now encounter is that (most) gimbals (especially inexpensive ones) are made for GoPros. Some more expensive ones can be balanced for any camera... but they are meant for much bigger and heavier cameras. We don't want to invent an all new gimbal for our drone. That's not our business and is way beyond our scope for this proof. So, how do we manage a Theta S on an existing gimbal for our proof of concept?

### Choosing the gimbal

As we've already said, the gimbal's main purpose is to keep the camera stable. It does this with the help of two methods:

1. Dampen vibration through rubber grommets (between the main frame and the gimbal mount).
2. Counter the normal tilting, rolling, and yaw movement of the drone via brushless motors on each axis.

Vibration dampening, we're all good. The same grommets are used for huge cameras as the smallest (just the count of grommets may change). Where our difficulty lies is in the balancing of the gimbal itself. We'd like to use a GoPro gimbal (as it's nearly the same weight—125 grams for the Theta S and 149 grams for the GoPro). Although the Theta S is (technically) lighter, the weight distribution will be completely different. It's a long, thin camera (that we'll likely have to hang upside down). But, because the dimensions are completely different, we can't ignore the principle of leverage on the gimbal axis. Remember, give me a lever long enough, and I can move the world?

We'll need to make sure that our gimbal can overcome the extra (effective) weight due to leverage, and keep our Theta S stable. This means it'll also need to be somewhat programmable to keep it properly calibrated.
