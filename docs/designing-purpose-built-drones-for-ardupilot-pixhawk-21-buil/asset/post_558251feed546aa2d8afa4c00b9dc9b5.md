
## 8

## The Principles of VTOL with Pixhawk

Designing and tuning a **Vertical Takeoff and Landing (VTOL)** aircraft can take a very long time. It may even take years to design and get your VTOL aircraft set up and working with stability. Originally, this chapter was going to be based on something fun—a *Battlebot* design. But, when the book was announced, one thing was very clear. You (the readers) wanted to know more about VTOL. So, without the time nor budget to create a VTOL prototype aircraft (by the time we have made one work properly, it's likely a new version of Pixhawk would be released); we're going to work on the principle and theory of how someone would create a Pixhawk VTOL; not how someone did create a Pixhawk VTOL.

In this chapter, we will cover the following topics:

- The types of VTOL aircraft
- Limitations and considerations of VTOL
- Fusing a multicopter and fixed wing design
- Tuning a VTOL aircraft

## The types of VTOL

There are essentially two types of VTOL: hybrid and vectored. All other VTOLs are a variation of the two. One is far easier to build, and one flies much better (and more efficiently). Unfortunately though, these are not the qualities of the same type of VTOL.

## The hybrid VTOL

This is certainly easy to design and build. It's just a matter of taking a very good airplane and attaching a multirotor frame to it (centred around the CG of the airplane). That's a bit of an oversimplification, but accurate. Unfortunately, with this type of design the multicopter propellers themselves can begin spinning during forward flight due to the wind. And even with no spinning they are essentially control surfaces and can cause strange turbulence and vortices around the wings, which alters the way it flies. This, plus having surfaces designed around moving air that are non-functional during forward flight, will reduce your efficiency. And (in general) making a fixed wing drone is about trying to gain more flight time (and possibly speed). So creating wind resistance is counter-productive. The following image shows a hybrid VTOL aircraft:

![](img/504adb30bdfce87f38258e08f11180af_223_0.png)

## Thrust vectored VTOL

Thrust vectoring is just a fancy word for changing the direction of the air that the thrust system is pushing. Whether it's by rotating propellers (like the Osprey military aircraft), rotating jet vents (like a Harrier), or changing the whole direction of the aircraft (like the X-Plane drone, which lands and takes off on its tail as a quad, and rotates to fly forward), the way to recognize a thrust vectored VTOL is that it uses the same thrust system to land and take off vertically as for pushing the aircraft forward during flight.

It's much more difficult to get this right during the design phase, from changes in the CG to taking into account how pushing or pulling an airplane through the air affects the airplane mode flight in different ways. It's certainly a design challenge, and you need to have a budget. Count on several crashes during the transition to forward flight on this type of aircraft until you get it right.

The reward for all of your effort with a vectored thrust VTOL is that every bit of weight is utilized during all modes of flight. You don't have any *non-functional* parts just hanging out during airplane flight. This also means no wasted energy. Whether that's pushing parts through the wind that aren't used during forward flight, or using control surfaces to counteract the way they affect your flight. Every time you (or your flight controller) uses a control surface, that means resistance from the wind, and of course, that means reducing the flight time.

The following image is a Harrier. The US (and British) Navies use this aircraft heavily due to its ability to land and take off from much smaller aircraft carriers that are far less expensive to build than the larger ones that carry aircraft that require a catapult to launch, and trap wires to land. The Harrier uses a large turbofan jet engine, and vents the exhaust of this engine to nozzles under the wings (on the sides of the main hull) that rotate, and have louvers to even push thrust to the sides. It's a very typical example of a vectored thrust VTOL aircraft, and one of the first:

![](img/504adb30bdfce87f38258e08f11180af_224_0.png)

The Hawker Harrier; image courtesy of Wikimedia commons

## The challenge

Luckily, we know just how much the plane already weighs wet (wet is with battery). That weight is 34.79 ounces (986.28 grams). What we need to do is find multicopter motors, ESCs, spars, and wiring that is under the flying weight limit for a multicopter with the total wet weight (of plane and additional parts) with enough juice left in the battery to maintain forward flight for a reasonable amount of time, and then land again vertically. We also need to stay below the maximum weight of the Bixler.

Our first problem arises. We're (technically) already above the maximum weight of a Bixler. The published maximum flying weight is 650 grams (22.92 ounces). However, this appears to be a dry weight. Even so, we're already pretty much at the maximum flying weight of the Bixler. So, it's a non-starter. We need a plane with bigger wings, stronger wings, much stronger wings, and since we're going there we need something that can carry (and run on) 6S batteries: 10,000 mAh 6S batteries. This is a very common battery for moderate-to-heavy lift multicopters, and should suffice for our VTOL.

## The simple gets more complex

Well, our Bixler obviously wouldn't work. It's just not designed to carry much weight nor payload for our purposes. Just one 10,000 mAh battery weighs ~1,400 grams (49.39 ounces). That's more than our entire flying weight of the Bixler. We choose this battery because 6S (22.2V) batteries provide enough juice to lift heavy loads in a multicopter. And 10,000 mAh should provide plenty of flight time for our airplane as well.

So, we know what won't do the job. What will? Enter the Skywalker EVE-2000. For a final production model, of course we'd design something more purpose built. But this is a great airframe to test out a hybrid design. It has huge wings (2,400 mm wingspan), and has a maximum wet weight of 4,600g (162.26 oz.).

![](img/504adb30bdfce87f38258e08f11180af_228_0.png)

So, our idea is to go through the hull, and attach the spars directly onto the wood (replaced by carbon fiber) plate. We're going to need some hardware:

## Creating our shopping list

Figuring budgets is essential when prototyping. You may be making one for a client, investors, or we're pretty sure your significant other would be interested in how much it will cost. So, it's essential before actually creating your final design to figure out what parts you'll (most likely) need.

First off, a Skywalker EVE-2000 is $285 (USD) via fpvmodel.com. It's a PNP (plug and play) version (so it comes with all servos, motors, and propellers), so all you need to do is plug in your Pixhawk and peripherals, then fly.

So, a Pixhawk 2.1 ($215), pitot tube ($70), and rangefinder for landing ($159) bring our running total up to $729. We're going to go a different route for GPS.

Because (generally) we want mapping data to be very accurate, we'll use a GPS system that is augmented by **Radio Telemetry Kinetics** technology (**RTK**). RTK uses a stationary GPS station on the ground and uses relation data based on the transmissions to the air system to allow the air system to get a much more accurate approximation of its actual coordinates. Where GPS has an accuracy of around a 6-foot (2-meter) diameter, RTK is centimeter accurate. That's pretty wicked-accurate. Here's the one we'd use to keep our budget and weight down:

![](img/504adb30bdfce87f38258e08f11180af_229_0.png)

With a price tag of $650, this brings our running total to $1,379.

With all of the variables of a multicopter, we would stop here, get it built and flight tested and tuned, and then start implementing the multicopter. But we'll get into the testing process in a bit.

Let's take the multicopter from the motors back to the airframe to make sure it can lift everything.
