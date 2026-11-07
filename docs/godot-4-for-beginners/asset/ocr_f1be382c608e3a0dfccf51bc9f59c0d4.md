Remember to call the animate() function in the _physics_process() function:

```
func _physics_process(delta):
    apply_gravity(delta)
    standard_player_movement()
    double_jump()
    wall_slide(delta)
    animate()
    move_and_slide()
```

In the preceding code, we have again made sure to call all our custom functions to run repeatedly in the physics_process function.

Now, test the game and your player should animate! Our next step is to set up the pass-through platforms so that the player can jump up through a platform from below and press down to fall through the platform from above.

# Falling through platforms

We created two different physics layers on the TileMap in Chapter 6 so that the player can jump up and fall down through certain platforms. Because we made these tiles as one-way tiles, we can jump through them from below, with no need for coding.

Now, we need to set up the code that will allow our player to fall through these platforms when we are standing on them and pressing down. Our code should check whether the player is pressing down and then simply turn off Collision Mask number 3 (the pass-through layer). This means that the player will stop detecting collisions on the pass_through layer and will fall until it reaches a layer that it is detecting collisions for, which is the ground layer. This is demonstrated in the check_pass_through() function as shown here:

```
func check_pass_through():
    if Input.is_action_pressed("ui_down"):
        set_collision_mask_value(3, false)
    else:
        set_collision_mask_value(3, true)
```

As can be seen in the preceding code, a new function has been created named check_pass_through(). It then checks whether the user is pressing the down key and, if so, it sets the value of Collision Mask layer number 3 to false (disabled). If the player is not pressing the down key, the value will be reset to true (enabled).