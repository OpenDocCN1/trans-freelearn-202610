
The animate() function is responsible for switching between different animation states based on the player’s movement, health, and whether they’ve been hit. Let’s walk through each condition to understand what’s happening.

First, let’s look at the code for the idle state:

```
if velocity.x == 0 && velocity.y == 0 && hearts > 0 && isHit == false:
    animations.play("idle")
```

If the player is not moving (velocity.x == 0 and velocity.y == 0), still alive (hearts > 0), and hasn’t just been hit (isHit == false), we play the "idle" animation.

Next, let’s examine the hit state:

```
elif isHit:
    animations.play("hit")
```

If the player has been hit, we play the "hit" animation to show a reaction. This usually plays for a short time before reverting to other animations.

This is how we detect running to the right:

```
elif velocity.x >= 0 && velocity.y == 0 && hearts > 0:
    animations.play("run")
    animations.flip_h = false
```

If the player is moving horizontally to the right (positive x-velocity), not moving vertically, and still alive, we play the "run" animation and make sure the sprite is facing right (flip_h = false).

The code for running left is similar:

```
elif velocity.x <= 0 && velocity.y == 0 && hearts > 0:
    animations.play("run")
    animations.flip_h = true
```

This will be the same as the previous condition, but for moving left. We flip the sprite horizontally (flip_h = true) to face the left direction.

Here’s how falling is handled:

```
elif velocity.y > 0 and !is_wall_sliding && hearts > 0:
    animations.play("fall")
```

If the player is moving downward (velocity.y > 0), not sliding on a wall, and still alive, we show the "fall" animation.

The jumping logic looks like this:

```
elif velocity.y < 0 and jump_count == 0 && hearts > 0:
    animations.play("jump")
```

This condition checks whether the player is jumping upward (velocity.y < 0) and it's their first jump (jump_count == 0), which triggers the "jump" animation.

Here's the check for double jumping:

```
elif !is_on_floor() and jump_count > 0 and !is_wall_sliding:
    animations.play("double_jump")
```

If the player is in the air (not on the floor), has used at least one jump, and isn't sliding on a wall, we show the "double_jump" animation.

This handles wall sliding on the right:

```
elif is_wall_sliding and Input.is_action_pressed("ui_right"):
    animations.flip_h = false
    animations.play("wall_slide")
```

If the player is sliding on a wall and holding the right movement key, we play the "wall_slide" animation and face the sprite to the right.

And, finally, this is what we use for wall sliding (left):

```
elif is_wall_sliding and Input.is_action_pressed("ui_left"):
    animations.flip_h = true
    animations.play("wall_slide")
```

It is the same as for wall sliding (right), but for the left side.

Everything is working almost as we want it to. However, you will notice that the player gets stuck in the hit animation, and that is because there is no event that sets the isHit variable back to false. The final step is to return to the Mushroom scene, select the HurtPlayerZone node, select the Node tab, then the Signals tab, and connect the body_exited() signal. Code the signal function as shown here:

```
func _on_hurt_player_zone_body_exited(body: Node2D):
    if body.is_in_group("Player"):
        body.isHit = false
```

To be consistent, we should also call the isHit function from the Mushroom script:

```
func _on_hurt_player_zone_body_entered(body):
    if "Player" in body.name:
        body.hurt()
        body.isHit = true
```

The preceding code ensures that if the body that exited the HurtPlayerZone Area2D node is part of the group called "Player", the hit variable is reset to false. This means that when the player stops colliding with the Mushroom, the hit animation will also stop.

Now, everything is in place to provide satisfying visual feedback to the user when they are playing the game to alert them that they are in danger.

Our next step is to add a fun visual effect to the checkpoint flag. We will fire off a confetti cannon as a small celebration of finishing the level.

## Creating a confetti cannon effect

To reinforce the feeling of achievement and as an extra reward for completing the level, when the player touches the checkpoint flag, we will fire some multi-colored confetti from the flagpole. This will take the form of a particle effect, which we will set up as its own scene. Let’s get started!

We’ll begin by creating a new scene with Node2D as the root node. Rename the node to Confetti_Cannon, add a GPUParticles2D node as a child, and rename this to Blue, as this will represent the blue confetti. This is shown in Figure 10.9 as follows:

![](img/d34207e07c133a9e84d9caf1d479006c_286_0.png)

Figure 10.9 – The Confetti_Cannon node and confetti particles

Now, select the Blue confetti particles node. In Inspector, find the setting for Process Material and then add New ParticleProcessMaterial, as shown in Figure 10.10:

![](img/d34207e07c133a9e84d9caf1d479006c_287_0.png)

Figure 10.10 – Creating a new particle process material

Next, click on ParticleProcessMaterial (the word) to expand the menu of settings for the particle.

The confetti should spin (rotate) as it flies, but it shouldn’t move away from the player. Enable Rotate Y and Disable Z in the Particle Flags settings, as shown in Figure 10.11:

![](img/d34207e07c133a9e84d9caf1d479006c_287_1.png)

Figure 10.11 – Particle Flags settings

The confetti should move in an arc to mimic the way confetti behaves in real life. To achieve this, adjust the angle at which the confetti spawns. You can adjust the numbers to suit yourself—I have set the minimum angle to 209 and the maximum to 327, as shown in Figure 10.12:

![](img/d34207e07c133a9e84d9caf1d479006c_288_0.png)

Figure 10.12 – Setting the min and max spawn angles for the confetti

Make sure to set the Direction variables as x = 0.0, y = -1.0, and z = 0.0, as shown in Figure 10.13.

The confetti particles should also spread out, and not all head off on the same path. To spread them out along a range of angles, I chose a Spread value of 100, which will pick an angle between −100 and 100 degrees for each new particle.

Additionally, since confetti is usually an explosive event, we want to vary the Initial Velocity values of the particles. Both changes are shown in Figure 10.13:

![](img/d34207e07c133a9e84d9caf1d479006c_288_1.png)

Figure 10.13 – Varying the Spread and Initial Velocity settings of the confetti particles

To make the effect even more impressive, we can change the rate at which each particle spins as it moves through the level. This can be done by varying the Angular Velocity values. I've locked the rate at 280, as shown in Figure 10.14:

![](img/d34207e07c133a9e84d9caf1d479006c_289_0.png)

Figure 10.14 – Setting the variable rate of angular velocity for the particles

The confetti should also accelerate away from the center at a steady rate, and we can easily set this in the linear acceleration setting (that is, Linear Accel) as shown in Figure 10.15:

![](img/d34207e07c133a9e84d9caf1d479006c_289_1.png)

Figure 10.15 – Setting the linear acceleration for the particles

To prevent the confetti from getting too far away from the flag, we should increase Gravity in the Accelerations section, as shown in Figure 10.16:

![](img/d34207e07c133a9e84d9caf1d479006c_289_2.png)

Figure 10.16 – Increasing the gravity in the y direction for the particles

To control what the confetti particles look like, we head to the **Display** properties of **ParticlesProcessMaterial**. Now, vary the **Scale** values of the confetti particles—I have set the minimum size to 5 and the maximum to 20. For **Color**, I chose *sky blue*. These changes can be seen in *Figure 10.17*:

![](img/d34207e07c133a9e84d9caf1d479006c_290_0.png)

*Figure 10.17 – Setting the scale variety and the color for the particles*

The next step is to control how long these particles remain on screen and how they behave while they are alive. The settings for this can be found in the **Time** property. The default lifetime is 1 second, which is fine; however, we must turn on **One Shot** so that the confetti only fires once and does not keep spawning. You can vary **Explosiveness** and **Randomness**, or use the values shown in *Figure 10.18*:

![](img/d34207e07c133a9e84d9caf1d479006c_290_1.png)

*Figure 10.18 – Adjusting the Time properties for the particles*
