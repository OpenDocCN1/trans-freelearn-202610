
+   2. **Remote**: Just because an area allows RC aircraft does not mean it's safe to test a new aircraft (especially an autonomous one). You'll need to make sure that there aren't any major buildings, homes, or pedestrians in the area that could get damaged (or even killed) if it crashes or decides to fly away. With the instructions we give you in the following chapters, everything should work. But unfortunately, we don't live in the world of what should be.
+   3. **You are authorized to be there**: Don't go trespassing on someone's ranch to test out your aircraft. At best, you could get in some legal trouble. At worst (in some places) you may even get shot. It's always good to ask for permission from the owner if you're going onto private property.

With these requirements in mind, the following Google Maps images show an example of a bad flying field (left) versus a good flying field (right). The one on the left is A-Main Hobbies' flying field in Chico, CA-great for recreational flying; completely wrong for prototyping. The one on the right is Dick Scobee Airfield in George Bush Park in Houston, TX-in the middle of a giant park. This is not perfect (because there is a road running through it), but definitely doable:

![](img/504adb30bdfce87f38258e08f11180af_128_0.png)

The one on the right is where I generally fly nowadays (since moving out to Texas). The one on the left is where I flew (already vetted aircraft) when I lived in California. For testing, I had to drive several hours to a remote location.

I built this drone in 2011. I thought I was really smart. The maximum payload for takeoff is 3.2 kg. So, I packed it full of batteries because I wanted this drone to stay in the air for extended periods of time. It flew beautifully when I was on the sticks. The first time I tested it on autonomous flight, the wings (literally) blew right off the plane, and it turned into a very expensive lawn-dart. It was completely totalled. So, what happened?

I failed to take into account g-force. G-force is the force put on an object due to centrifugal force. It is measured in multiples of the standard gravity of the Earth when standing still, on the ground, at sea level. For instance, a 100 lb pilot taking a 2-G turn will weigh 200 lbs during that turn.

When I flipped a switch for the plane to return home, it accelerated to maximum throttle, banked to 45 degrees, and took a tight turn to come back home. This was probably a good 4-G turn. So, even though I was under the weight limit at takeoff (only 3.0 kg), during that turn the aircraft had forces of 12.0 kg put directly on the wings. This was enough to shatter the wings and begin the metamorphosis to lawn-dart. A very stupid, expensive, and easy oversight to make.

Weight can also affect flight time. This is much more severe with multicopters because multicopters use power to generate lift, but it's also true with fixed-wing drones. Why? Because weight affects your glide ratio. The glide ratio of a plane looks something like 1:100. This means that for every 100 feet of forward flight, it will lose 1 foot of altitude with no power. To overcome this, the nose is ever-so-slightly pointed up and power is applied. Increase the weight, and the distance a plane will travel before losing altitude with no power will decrease; therefore, increase the weight and increase the power that is necessary to keep the plane airborne. And therefore, the less flight time you will have before exhausting your power supply.

A multicopter's glide ratio is 1:0. This means it can't travel forward at all without losing altitude without power. Essentially, it's even less of a glide ratio than a piano. So weight greatly affects the flight time of a multicopter. We'll get deeper into that when we build a multicopter in Chapter 6, A Simple Multicopter Drone.

Finally, weight also affects the center of gravity (cg). Flying an aircraft is a bit like balancing a ball on the point of a pencil. An aircraft should be as balanced as possible to enable the control surfaces to work properly with the most efficiency (fixed wing), or to keep the motors from being out of balance, and potentially burning out one of them (on a multicopter). Either way, an unbalanced aircraft will handle poorly, to say the least.

On a fixed-wing aircraft, you may find that the cargo compartment (where battery, electronics, and payload are kept) is on the front of the airframe. Put too much too far forward, and you'll find that the center of gravity becomes nose-heavy. This means you may need to add more weight to the tail as ballast (counterweight). Generally, the cg for a fixed-wing aircraft is on the peak of the airfoil. Think of this as a fulcrum on a scale. You'll need to balance that scale. We'll get deeper into this in two chapters when we build a fixed-wing aircraft.

## Power/lift

There was a time when having a multicopter stay in the air for more than 5 minutes was considered a feat of engineering.

One of my multicopters from the past was built for small camcorders (barely larger than a GoPro). It's pictured next, as featured in Videomaker Magazine. I was asked by a client to mount a Blackmagic Cinema camera on it. Of course, it was designed to lift that much weight. But for how long? It only stayed in the air for a whopping 2 minutes at a time. Power and lift need to be carefully balanced with the weight and battery to keep (especially) a multicopter in the air:

![](img/504adb30bdfce87f38258e08f11180af_132_0.png)

## Fuel - battery

The simplest way to get more flight time is to add bigger batteries or more fuel, right? Wrong! If your drone is gas-powered, you have to keep in mind that fuel weighs 8 lbs per gallon and batteries give diminishing returns. For instance, using a battery that can power your motors 1X spec may weigh 2 lbs. But going to 2X spec may weigh 6 lbs. So twice the power rating on a battery doesn't mean twice the time because the motors have to work 3X as hard to keep that extra weight in the air!

## Learning some physics

Nooooo! (you're probably saying). Unfortunately, depending on what you're prototyping, kitbashing may not be much of an option. Because we are going to kitbash in our chapters (as we could write a whole other book per type of aircraft), let's give you a brief crash course in aeronautical engineering.

Designing an aircraft from scratch is not for the faint of heart. Multicopters are much easier, so let's cover that first.

## Designing a multicopter airframe

What, what in the heck is an airframe? It’s just a fancy word for the body of an aircraft. Why would you need to design your own? Well, if you're going to start a new company selling a saturated product (such as a camera drone), you may need to differentiate yourself with an entirely new-looking drone. After all, why would anybody buy your design on (say) a DJI airframe, when they can just go buy a DJI?

Designing a multicopter frame is fairly easy; there are just a few factors you need to take into account, which we will cover in the upcoming subsections.

### Symmetry

Keeping the motors driving the propellers equidistant is very important. Remember, a multicopter can fly in any direction equally well. To keep this important aspect of a multicopter, you must design your multirotor with motors in this pattern.

## Even numbers

There is an exception, namely tricopter (three rotors). Although these were fairly common about 5 years ago, they are notoriously unstable. Why? Because of how multicopters turn. More blades not only equal better stability and lift for a multicopter, even numbers make the calculations necessary for Pixhawk to keep it in the air easier. If you remember, back in Chapter 1, Drones 101, we covered the physics of how a multicopter's yaw works. Even numbers make this far more stable.

In conjunction with symmetry, let's lay out how many degrees the different blade configurations would be in the following table. Just imagine looking at the multicopter from before and dividing the sections like slicing a pie. If there are 360 degrees in a circle, here's how things would line up:

| Number of rotors | Degrees of separation |
|---|---|
| 3 | 120 |
| 4 | 90 |
| 6 | 60 |
| 8 | 45 |

## Blade clearance

The spars you design (the sticks that go from the center of the aircraft out to the motors) need to be long enough so that the blades clear any antennae and clear each other comfortably. So the more rotors that you have, the longer the spars need to be. The longer the spars are, the stronger they also need to be (due to the force known as leverage). Then, you have to understand that the longer and stronger the spars, the more weight you add in airframe and wiring for the motors.

With all of the preceding factors in mind, here is an example of an 8-rotor multicopter (octocopter) with 22-inch propellers:
