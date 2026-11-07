An image of the flag_fly animation is shown as an example in Figure 7.20:

![](bbox=[0.13, 0.12, 0.87, 0.34])

Figure 7.20 – Setting up the flag animations for the checkpoint

Attach a script to the CheckPoint node to customize its behavior. We will create our own signal to trigger the flag to fly out. Once the player has collected all of the Strawberries in the level, the player will emit the trigger signal. The CheckPoint code is shown and explained as follows:

```
extends Area2D
signal trigger
var level_complete = false

# signal that runs when the animation is finished playing
func _on_animated_sprite_2d_animation_finished():
    $AnimatedSprite2D.play("flag_fly")
    level_complete = true

# our own custom signal
func _on_trigger():
    $AnimatedSprite2D.play("trigger")

# A signal that runs when a body enters
func _on_body_entered(body):
    if level_complete:
        get_tree().quit()
```