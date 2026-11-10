# How it works...

We are using a similar principle as used in the *Using root motion to create flying characters* recipe in `Chapter 4`, *Character Movement*. Our script calculates the angles between our target and our character's forward axis. Then it assigns those angles as parameters in the controller. The Blend Tree uses those parameters to blend between nine directional aim animations.

# There's more...

You can further develop this recipe with additional animations to handle more extreme angles. You can even have animations targeting +180 and -180 degrees in horizontal angles. This would make your character aim in every possible direction.

# Detecting the hit location on a character

Another animation-related topic in combat is the hit location detection. This recipe shows how to easily detect hits to different body parts.

# Getting ready

We are going to use a simple character with just one looped **Idle** animation in this recipe. You can also use the provided example Unity project and go to the `Chapter 06 Handling combat\Recipe 10 Detecting the hit location on a character` directory. You will find an `Example.unity` scene there, with a **Humanoid** character. Play the game to see the effect.

# How to do it...

To be able to detect hits for different body parts, follow these steps:

+   1. Import your character with all the necessary animations to Unity. Place it on the scene.