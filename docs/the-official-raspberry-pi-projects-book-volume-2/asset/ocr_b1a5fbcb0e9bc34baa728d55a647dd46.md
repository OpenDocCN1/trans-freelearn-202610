# SOCRATIVE
ZOMBIE

A philosophical zombie, or a frightening, brain-munching head?

DAN ALDRED
Lead schoolteacher for Computing
At School, raiser of the dead
magpi.cc/2dqFmWC

It's gruesome, even in the light of day

![](bbox=[0.44, 0.06, 0.99, 0.53])

![](bbox=[0.78, 0.47, 0.95, 0.52])

Out late at night, you should have known better than to enter the strange shed, but it was about to rain and you needed the shelter. A sense of unease comes over you as the door closes behind you. It’s probably just because it’s a bit of a creepy atmosphere. The winds begin to howl outside, but wait – that’s not a howl, it’s more of a gurgling groan. And it’s coming from inside the shed with you. As you turn to confront the noise, you see an eerie glow from the corner of your eye. You fumble for your phone and turn the torchlight on to see the source of the noise and light… only to find an animated, undead head hungry for your flesh.

Below The inside of the zombie is a less creepy affair, as you can see the parts that make it work

![](bbox=[0.07, 0.65, 0.42, 0.94])

Luckily, it’s just a sculpture from a school project, but you get the picture. Its creator Dan Aldred tells us how he came up with such a horrifying idea:

> > “I wanted to create a Halloween hack that would scare people. Sheds are scary in the dark, but even more with a talking zombie head! I also wanted to create a interesting head that would ask my students questions related to their learning.”

We assume they learnt the true meaning of fear alongside their new-found coding and soldering skills. The head is one of the few projects in this feature that makes use of sensors to know when someone is approaching it. In this case, a PIR motion sensor detects changes in temperature that correspond to a warm body entering its field of view. This means it unfortunately would not be able to sense its fellow undead. The sensor is placed in the mouth and controls a Python script which also hooks into LEDs and a speaker to complete the effect.

If you plan to replicate this project, Dan recommends a Raspberry Pi A+ due to its small size, and to create a cron job to start the program once the Raspberry Pi is powered up. How did it fare on the night?

> > “I had it in the classroom, it got dark about 4pm, and the students were very intrigued and excited,” Dan tells us. “Then I moved it to my shed for the 31st and when the trick-or-treaters came around, they went in the shed and were scared. It made them jump; they really liked the zombie head.”

# COMPONENTS LIST

CONJURE UP YOUR OWN VERSION

+   - Zombie head
+   - PIR infrared motion sensor
+   - LEDs
+   - Speakers