Now, click on the word SpriteFrames in the property to open the Animation Frames window. Rename the default animation to idle and add the four frames, as shown in Figure 10.5:

![](bbox=[0.13, 0.15, 0.87, 0.29])

Figure 10.5 – The idle animation for the health bar has four frames for the health total

As you can see in Figure 10.5, whenever the player loses health, we will change the animation frames to match. We don’t need to add a script to the health bar; all we need is a reference to it in the script attached to level1.tscn.

In the Level1 scene, drag your newly created hud.tscn into your level and place it toward the top left, as shown in Figure 10.6:

![](bbox=[0.13, 0.47, 0.87, 0.82])

Figure 10.6 – A good spot for the HUD