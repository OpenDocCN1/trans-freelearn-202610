# Even numbers

There is an exception, namely tricopter (three rotors). Although these were fairly common about 5 years ago, they are notoriously unstable. Why? Because of how multicopters turn. More blades not only equal better stability and lift for a multicopter, even numbers make the calculations necessary for Pixhawk to keep it in the air easier. If you remember, back in Chapter 1, Drones 101, we covered the physics of how a multicopter's yaw works. Even numbers make this far more stable.

In conjunction with symmetry, let's lay out how many degrees the different blade configurations would be in the following table. Just imagine looking at the multicopter from before and dividing the sections like slicing a pie. If there are 360 degrees in a circle, here's how things would line up:

| Number of rotors | Degrees of separation |
|---|---|
| 3 | 120 |
| 4 | 90 |
| 6 | 60 |
| 8 | 45 |

# Blade clearance

The spars you design (the sticks that go from the center of the aircraft out to the motors) need to be long enough so that the blades clear any antennae and clear each other comfortably. So the more rotors that you have, the longer the spars need to be. The longer the spars are, the stronger they also need to be (due to the force known as leverage). Then, you have to understand that the longer and stronger the spars, the more weight you add in airframe and wiring for the motors.

With all of the preceding factors in mind, here is an example of an 8-rotor multicopter (octocopter) with 22-inch propellers: