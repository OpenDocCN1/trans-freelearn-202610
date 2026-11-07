Let’s begin by adding a new script to the CharacterBody3D node of the Player scene. Make sure that Template is unchecked for Empty so that we do not get any pre-made code and can make a completely custom character controller as shown in Figure 8.15:

![](bbox=[0.32, 0.17, 0.68, 0.43])

Figure 8.15 – Adding an empty script to the player

When the Template check box is not checked, as in Figure 8.15, we are presented with a script that only has one line:

```
extends CharacterBody3D
```

This line means that this script is adding to or customizing the built-in CharacterBody3D node in Godot. The CharacterBody3D node is used to create 3D characters, such as players or enemies, which can move, jump, or interact with the environment. By extending it, we can write our own code to make the character behave exactly how we want.

Although we added a camera to our Player scene, this won’t be the only camera used during gameplay. Instead, we’ll set up a second static camera in the level that we can rotate to view the player from different angles. In Godot, a scene can include multiple cameras, and you can choose which one is active by marking it as Current in Inspector.

# Using variables

Before we dive into the code, let’s take a moment to understand what it’s doing. In this script, we’ll define a few important properties that control how our player character moves, such as how fast they walk or how high they can jump. We’ll use exported variables, which means these properties will not only be defined in code but will also be visible and editable directly in the Godot editor’s Inspector panel.