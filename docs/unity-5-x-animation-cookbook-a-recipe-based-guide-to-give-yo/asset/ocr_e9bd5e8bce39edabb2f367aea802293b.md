# There's more...

Sometimes we need to check not only the current state of the Animator but also a specified moment in it. Let's assume we have a 2-second long jump attack animation. Our character starts the jump in the first second of the animation, and then it jumps and lands on the ground with a strike at the end of the animation. We may want to allow players to break the animation when the character is still on the ground (for instance, with a dodge animation). To do so, we may use Animation Curves to set an AllowBreak float parameter (you can choose a different name) in the controller and check this parameter value while trying to perform actions or transition to other states. Animation Curves are described in the Using animations for better looking transitions recipe in Chapter 4, Character Movement.

# Using Animation Events to draw a weapon

Another common case in combat mechanics is drawing and sheathing a weapon. In this recipe we will use Animation Events along with a Draw animation to make our character draw and sheath a sword.

# Getting ready

To follow this recipe, we need to have a Warrior character with two animations: Idle and Draw. The first one is a standard, looped Idle animation. The second one is an animation in which our character reaches for their weapon in a sheath attached to the character's belt. We are not using a sheath model here to make the recipe more simple and condensed. You can open the provided example project and go to the Chapter 06 Handling combat\Recipe 06 Using animation events to draw a weapon directory. There is a Warrior character in the Example.unity scene. When you press the space bar, it will draw or sheath a sword. You can find all the necessary animations in the Rigs directory.

# How to do it...

To make a character draw or sheath a weapon, follow these steps:

+   1. Import the Warrior character with the Idle and Draw animations into Unity.
+   2. Import a Sword prop to Unity.
+   3. Go to the Import Settings | Animation tab. Select the Draw animation and navigate to the Events section.