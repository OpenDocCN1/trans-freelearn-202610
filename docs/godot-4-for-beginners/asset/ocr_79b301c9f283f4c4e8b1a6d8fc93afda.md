The final custom function for our player will handle respawning and animation effects upon landing. Create the handle_respawn() function as shown in the code:

```
func handle_respawn(delta):
    var border_position_y = -10
    # Threshold below which the player respawns
    if position.y < border_position_y:
        get_tree().reload_current_scene()

    # Animation for scale (jumping and landing)
    model.scale = model.scale.lerp(Vector3(1, 1, 1), delta
    * smoothing_factor)

    # Animation when landing
    if is_on_floor() and vertical_velocity > 2 and
    !previously_floored:
        model.scale = Vector3(1.25, 0.75, 1.25)

    previously_floored = is_on_floor()
```

Let’s investigate the handle_respawn() function:

+   - **Scene reload (respawning):** If the player’s position.y falls below -10 (meaning they’ve fallen off the level), we call get_tree().reload_current_scene() to reload the current scene and respawn the player at the starting position.
+   - **Resetting scale (restoring size):** We use model.scale.lerp(Vector3(1, 1, 1), delta * smoothing_factor) to gradually interpolate the player’s scale back to normal after any squash-and-stretch effects from jumping or landing.
+   - **Landing animation (squash effect):** If the player is currently on the ground (is_on_floor()), was falling fast (gravity > 2), and was not on the ground in the previous frame (!previously_floored), we trigger a squash effect by changing model1.scale to Vector3(1.25, 0.75, 1.25).
+   - **Grounded state tracking:** Finally, we update the previously_floored variable to keep track of whether the player was on the ground during the last frame.