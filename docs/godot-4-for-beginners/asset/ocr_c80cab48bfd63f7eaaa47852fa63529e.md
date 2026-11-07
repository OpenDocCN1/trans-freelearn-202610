![](bbox=[0.36, 0.09, 0.64, 0.37])

Figure 8.14 – The individual scenes making up the level

**Grouping** is important because it keeps the scene tree organized, especially for larger levels. It allows you to move or manipulate entire categories of objects together, which can save time and reduce errors. For example, if you want to disable all fences or apply a shader to all grass, having them grouped makes this much easier.

It’s also worth noting that most of these objects, such as the ground, fences, rocks, and bridges, are static. They don’t move during gameplay and should be implemented using **StaticBody3D** for better performance and accurate physics.

In contrast, dynamic objects such as the player are designed to move and react to the environment and are therefore implemented using physics-enabled nodes such as **CharacterBody3D**.

This kind of structure not only helps with level design but also ensures smooth performance and cleaner code management as your project grows.

As shown in Figure 8.14, we now have a complex **Level 1** scene that is made up of many smaller scenes. Although we have a player in the scene, there is no way to control it. In the next section, we will work on the Character Controller script.

# Working with a Character Controller script

In this section, we will write a script that allows players to move the character around the 3D world. This script is essential because, without it, our character remains static – unable to walk, run, or jump through the level. By the end of this section, you’ll have a fully controllable player character that can respond to input and interact with the game environment in a meaningful way.