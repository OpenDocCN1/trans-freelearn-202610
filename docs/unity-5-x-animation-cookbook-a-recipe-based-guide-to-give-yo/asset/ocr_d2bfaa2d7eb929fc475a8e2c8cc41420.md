# How it works...

This recipe animates the color of an Image component (a part of Unity's UI). We can animate the alpha parameter to fade the transparency of an Image. It allows us to create a fade in/fade out effect very easily. This recipe has some other key elements:

+   - **FadeAnimator empty game object**: Our **BlackScreen** game object (containing the **Image** component) is a child of an empty game object (named **FadeAnimator** in this example). The empty game object has the Animator component attached. This way we can disable and enable the **BlackScreen** game object in our animations (it wouldn't be possible if the Animator component would be attached to the same **BlackScreen** game object).
+   - **Disabling and enabling the BlackScreen game object**: It is a good practice to disable our black screens because they can block mouse input for UI elements if they are enabled (even if they are fully transparent).
+   - **BlackScreen in the bottom of the Hierarchy**: Our **BlackScreen** object should be placed in the very bottom of the **Hierarchy** to cover all other UI elements in our game.

# Creating a swipe transition with the Animation View

In this recipe, we will create a bit more creative transition effect. A lot of the steps are similar to the previous recipe, but the final result is completely different. This recipe's goal is to encourage you to experiment with animating different UI properties to achieve interesting effects.

# Getting ready

Similar to the previous recipe, we will create everything from scratch in Unity; you don't have to prepare anything beforehand. You can open the provided example Unity project and go to the `Chapter 03 2D and user interface animation\Recipe 06 Creating a swipe transition with the animation view` directory. You will find an `Example.unity` scene there. Play the game and press space bar to see the swipe transition effect.