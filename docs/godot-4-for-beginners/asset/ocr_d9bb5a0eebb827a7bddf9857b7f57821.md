# Technical requirements

By this point in the book, you should know how to do the following:

+   - Create nodes
+   - Create scenes

You should also know about variables and functions for use in GDScript (see Chapter 4).

This chapter’s code files are available here in the book’s GitHub repository: https://github.com/PacktPublishing/Godot-4-for-Beginners/tree/main/ch7

Visit this link to check out the video of the code being run: https://packt.link/goVK2

The game assets used in this project are released under a Creative Commons Zero (CC0) license by Pixel Frog. Pixel Frog has allowed us to distribute, remix, adapt, and build upon the material in any medium or format, even for commercial purposes.

They can be found here:

+   - Pixel Adventure 1: https://pixelfrog-assets.itch.io/pixel-adventure-1
+   - Pixel Adventure 2: https://pixelfrog-assets.itch.io/pixel-adventure-2

# Controlling player animations with code

Animations play a crucial role in bringing your game to life, making player actions feel smooth and responsive. In this section, we'll explore how to trigger different player animations in code. By learning how to control animations programmatically, you'll gain the ability to seamlessly transition between movement states such as idle, running, and jumping, enhancing the overall feel and polish of your game.

Take a moment to consider the conditions that will trigger each animation that we created in the previous chapter. We should try to imagine what will cause the animation to play so that we can implement it in the code:

+   - The idle animation is triggered when the player is stationary (both x and y velocities are 0).
+   - The run animation is triggered when the player moves left or right. To ensure the sprite faces the correct direction, we'll flip it based on movement direction.
+   - The fall animation is triggered when the player is moving downward (the y velocity is increasing) and is not wall-sliding.
+   - The jump animation plays when the player is moving upward (the y velocity is decreasing) during their first jump (to prevent the player from spamming jump to float or fly).