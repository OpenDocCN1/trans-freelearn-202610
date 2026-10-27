
![](img/504adb30bdfce87f38258e08f11180af_30_0.png)

The similarities between the Navy's "Ocean Glider" submersible drone and the original X-1 airplane (the airplane which first broke the sound-barrier) are rather astounding.

## UAV drones

**UAV** stands for **Unmanned Aerial Vehicle**. Simply stated, this is any drone that flies. However, much like the term UUV can also include submersibles, this category can also cover vehicles that go into space (such as the Pioneer satellites, the Cassini probe, or even the Air Force's replacement for the Space Shuttle—the X-37B).

The US Air Force's X-37B can be drop-launched from a high-flying airplane, or mounted to a rocket. It can also autonomously stay in space for several years, change orbital paths, and return to the ground for an autonomous landing. The actual payloads and missions are classified. You didn't really think we'd rely on Russia and civilian agencies for all the United States' needs in space, did you?

We won't bother to get into space any further. After all, if you're working for NASA you probably don't need this book. Still, it was cool to mention. In the air, UAVs can be broken into five essential categories:

## Multirotors

Sometimes, people refer to these as *quadcopters* or *quadrocopters*. Technically, this isn't accurate unless the multirotor has four-propellers. There are also hexacopters (six-propellers), octocopters (eight-propellers), and even all the way up to thirty-two-propellers (although I have no idea what that word would even be). So, you can see how multirotor simplifies it all. After all, they are essentially the same type of aircraft, just with more or less motors and propellers. (It's like calling a monoplane and a biplane an airplane, but calling a monoplane a *biplane* is incorrect).

### How do multicopters fly?

The principle is simple. So simple (in fact) that you may wonder why these weren't a thing until recently. Propellers blow air down to provide lift and the airframe (the whole body) tilts to direct that thrust behind (to go forward), in front (to go backward), or to either side (to slide left or right). You can see this in the following illustration:

![](img/504adb30bdfce87f38258e08f11180af_31_0.png)

But then there's controlling it.

It requires thousands of calculations per second to decide exactly how much power should go to each motor to keep the aircraft level and stable in the air. No two motors come out exactly the same. So, it's not just a matter of providing equal voltage to each motor. It's a matter of adjusting that voltage to each motor based on what's currently happening to the aircraft. So, in essence the aircraft doesn't fly; it reacts to its inability to fly and compensates for it. And then there's turning (yaw).

You may notice there's no tail rotor on a multicopter. Tail rotors on traditional helicopters serve two functions:

- Keep the airframe (the body) of the aircraft from spinning out of control. After all, Newtonian Physics tells us that for every action, there is an equal and opposite reaction. If the motor is turning the primary rotor, what's to stop it from spinning the body in the opposite direction?
- By varying the pitch of the blades on the tail rotor, more or less thrust (as well as the direction of the thrust) can be controlled. This lets the pilot yaw the aircraft (turn it).

So, how does a multirotor achieve yaw? The answer is in the preceding first reason. There are (usually) even numbers of rotors on a multicopter. Half of these rotors spin in one direction (clockwise) and the other half in the other direction (anti-clockwise). When you want to turn left, the blades spinning to the right speed up and the blades spinning to the left slow down. This way, the effective lift remains the same but the torque from the motors yaws the aircraft to the left. The opposite happens when turning to the right. The following illustration shows this in action:

![](img/504adb30bdfce87f38258e08f11180af_32_0.png)

This is also why tricopters (three rotors) are largely not used. They were notoriously unstable and battery-inefficient. They looked super cool though. Rather like the drop ship from the movie Aliens. However, a variation of the hexacopter (six-rotors) uses that motif (which brings us to yet another variation on blade-configuration). A flat-hexacopter has all-six blades stationed on a single plane. The over-under version of a hexacopter looks like a tricopter with three basic lift points, but counter-rotating blades stacked atop one another so one plane of blades moves in one direction; while the one following moves opposite.

The following picture shows a standard quadcopter (upper left), an over-under hexacopter (upper-right), and a flat-hexacopter (bottom). Most configurations for multicopters are now flat, as it's more efficient for battery time and lift capabilities:

![](img/504adb30bdfce87f38258e08f11180af_33_0.png)

So, as you can see, multicopters are a platform based on a simple principle that is extremely complicated to make work. You (the pilot) don't actually fly a multicopter. It flies itself. Instead, you really just tell it where to go and what to do. You don't pilot a multicopter, you wrangle it.

For the guidance system (in our case we'll be using Pixhawk 2.1—an Ardupilot-based system) to accurately calculate what each motor should be doing there is a plethora of sensors it needs:

- **Accelerometers**: Measure the attitude of the aircraft to see whether it's level, what direction it's moving in (relative to last attitude). These essentially measure the linear velocity. So, there need to be several of these sensors in order to measure all axes of movement. The Pixhawk has three accelerometers which are quite good and very fast. However, more can be added.
- **Gyroscopes**: These differ from accelerometers in that they sense angular movement (roll, pitch, and yaw). Each gyroscope measures one axis of movement, so three are needed. Pixhawk includes three gyros.
- **Magnetometers**: This sensor functions essentially as a compass. They measure the Earth's magnetic field and can tell the vehicle's relative heading to north. However, as aircraft can roll and pitch, one should be implemented for each axis and compared to each other (as one magnetometer may get locked if perpendicular to north). Pixhawk has three of these.
- **Barometers**: A barometer measures air pressure. Because air pressure changes at various altitudes, this method can tell the relative altitude to the takeoff point. Amazingly, these can be quite accurate (down to individual inches) depending on the number of sensors, and their sensitivity. Pixhawk has two built in.
- **Global positioning system (GPS)**: GPS tells the Pixhawk where it is (on the planet). We'll get deeper into GPS and how it works in a later chapter, but let's just say that not all GPS receivers are created equally. GPS is an approximation and various factors determine how accurate it is. GPS is not included with Pixhawk. You must buy your own GPS module for it.

So, accuracy of sensors is extremely important (as you can see) with regard to flying multicopters. If it's truly flying itself and you're just telling it where to go, you want that robot brain to have as much information (which is as accurate) as possible.

The myth with multicopters is that they're extremely simple. This is probably because they are fairly easy to fly. That myth should be dispelled right here and now. Of all the drone-types, they are the most difficult to design and build. You'll find that parts should work together, but don't. You'll find that balancing lift to flight-time is extremely difficult. And you'll find that when things go wrong, they go very wrong. Trust me, I have the scars to prove it. So, when we get into multicopters, please pay the utmost attention to safety. They are essentially flying Cuisinarts.

## Fixed-wing drones – airplanes

Airplanes are considered by many as the Holy Grail of UAVs. They can generally fly for a lot longer than multicopters (as they have wings and don't have to use up battery power on lift). They are much easier to fly (as they can actually glide and, if the motor cuts out, they don't drop like a rock). However, the difficulty with airplanes is takeoff and landing.

Since airplanes don't hover, they must be moving at speed to take off and land. Landing an airplane isn't too easy either. They must touch the ground softly, or they may bounce up again, stall, and crash. So, how do you design a drone to land at speed softly? It's not easy. But Pixhawk has been known to do this pretty well.
