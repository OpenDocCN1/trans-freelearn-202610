```
# Only allow jumping if the player is currently on the ground
if Input.is_action_just_pressed("ui_accept") and is_on_floor():
    velocity.y = JUMP_VELOCITY

# This function handles horizontal movement input
func handle_movement(delta):
    # Get the left/right movement input as a value between -1 and 1
    var direction = Input.get_axis("ui_left", "ui_right")

    # If there is movement input, update the velocity in that direction
    if direction:
        velocity.x = direction * SPEED
    else:
        # If no input, gradually slow the player down to a stop
        velocity.x = move_toward(velocity.x, 0, SPEED)
```

The code functions in the same way as before, but now specific actions reside in their own functions, so it is easier to find errors and make improvements.

The code is now clean and broken down into smaller and more maintainable parts. In the next chapter, we will be customizing more of the players’ behaviors and playing the animations. We will also add a collectible, an enemy, and a checkpoint to complete the level.

# Summary

In this chapter, we covered essential steps in building your first 2D platformer level in Godot. We started by setting up a TileSet within the TileMapLayer node with collision layers and painting the level using the TileMap system. From there, we created the player character, importing the necessary animated sprite frames for different animations. With the character in place, we implemented a default script using Godot’s CharacterBody2D template. Finally, we took the time to clean and organize the code by splitting it into individual functions, a key part of following the clean code principles.

In the next chapter, we will animate the player and code special abilities such as double-jump and wall sliding. We will also set up collectible items, an enemy to defeat, and a checkpoint to reach to finish the level.