The mushroom_death() function should be called in the _physics_process() function, as shown in the code:

```
func _physics_process(delta):
    add_gravity(delta)
    moveEnemy()
    move_and_slide()
    reverse_direction()
    platform_edge()
    mushroom_death()
```

Connect the _on_animated_sprite_2d_animation_finished() signal of the AnimatedSprite2D node of the Mushroom. When the death animation is finished, it will trigger the function to remove the Mushroom from the scene, as shown in the code:

```
func _on_animated_sprite_2d_animation_finished():
    if $AnimatedSprite2D.animation == "death":
        queue_free()
```

Add some more Mushrooms and collectibles to your level. To prevent crowding the scene tree, add a new child node of the Node type. Rename it Enemies and drag all of the Mushrooms under it. You can do the same for Collectibles, as shown in Figure 7.18:

![](bbox=[0.28, 0.54, 0.7, 0.87])

Figure 7.18 – Organizing the scene tree by grouping nodes