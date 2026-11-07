Remember to add check_pass_through() to your list of function calls in the _physics_process(); otherwise, the function will not run!

```
func _physics_process(delta):
    apply_gravity(delta)
    standard_player_movement()
    double_jump()
    wall_slide(delta)
    animate()
    check_pass_through()
    move_and_slide()
```

The player can now run, jump, double-jump, and wall-slide around the level as well as fall through special platforms. Our next step is to scatter some items around the level for the player to collect.

# Adding collectible items

In many games, **collectible items** play a vital role in enhancing gameplay by providing rewards, power-ups, or essential resources to the player. These items can create a sense of progression, encourage exploration, and add layers of strategy to the game. In this section, we’ll explore how to create a collectible item system in Godot.

You’ll learn how to design a collectible item, detect when the player interacts with it, and implement logic to grant rewards or effects. By the end of this section, you’ll have a reusable system that can be extended to include a variety of collectibles for your levels.

For the collectible item, we will use the Strawberry found in the Items | Fruits folder. However, you may use any item you wish as long as you adapt some of the steps that follow.

# Strawberry scene (our collectible item)

For our game, the player will collect Strawberries. Once the player has collected all of the Strawberries on a level, a flag will appear which the player will touch to complete the level.

Follow these steps to create the collectible item scene:

+   1. Create a new scene and add **Area2D** as the root node. **Area2D** is an invisible node that defines a region of 2D space, which detects when objects have collided or stopped colliding with it.
+   2. Add an **AnimatedSprite2D** node as a child node.