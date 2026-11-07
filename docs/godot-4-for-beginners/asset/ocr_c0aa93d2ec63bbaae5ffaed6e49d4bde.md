+   - UI updates
+   - Non-physics-related updates

Note that delta is the amount of time (in seconds) since the last frame. This helps you keep movement smooth even if the frame rate changes.

# _physics_process(delta)

This is like _process, but it runs at a fixed rate (by default, 60 times per second). This makes it perfect for anything involving physics, such as moving a character with collision.

Use _physics_process(delta) for the following:

+   - Physics calculations
+   - Movement with move_and_slide() or move_and_collide()
+   - Gravity and jumping

To demonstrate how the physics_process function works, follow these steps:

1. Create a new scene with Sprite2D as the root node, as shown in Figure 4.5:

![](bbox=[0.38, 0.44, 0.68, 0.56])

Figure 4.5 – A Sprite2D node as the root of the scene

2. Use the Godot icon.svg file as the texture for the sprite.

3. Attach a script to the node by right-clicking on it and choosing Attach Script... or clicking on the script icon with the + symbol. Add the following code to the script:

```
extends Sprite2D
var speed = 100
var velocity = Vector2.ZERO

func _ready():
    print("Sprite is ready to move!")

func _process(delta):
```