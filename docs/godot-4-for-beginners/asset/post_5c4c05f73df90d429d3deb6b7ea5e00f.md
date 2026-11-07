
4. Drag the cloud_1.obj file into the Mesh property. This is shown in Figure 9.14:

![](img/d34207e07c133a9e84d9caf1d479006c_263_0.png)

Figure 9.14 – Using one of the clouds as Mesh to represent smoke

5. Remember to texture the mesh by applying the palette.png texture.

6. Expand the Gravity section and make the y value positive. Once you do this, the smoke rises.

7. Reduce the Amount property to 1, so that a puff of smoke will rise from the cannon.

8. Set the One Shot property to On. This is shown in Figure 9.15:

![](img/d34207e07c133a9e84d9caf1d479006c_263_1.png)

Figure 9.15 - Changing the amount and setting One Shot to On

9. Orient the cloud vertically by expanding the **Transform** section of **Node3D** and rotating the z axis by 90 degrees. This is shown in Figure 9.16:

![](img/d34207e07c133a9e84d9caf1d479006c_264_0.png)

Figure 9.16 – Rotating the model on the z axis to make it vertical

10. Adjust the scale of the cloud over time, from 0 to full size (1). Do this by expanding the **Scale** section and setting **Scale Amount Minimum** to 0 and **Scale Amount Maximum** to 1.

11. Create a **Scale Amount** Curve by clicking on **New Curve** and adjusting the scale over time, as shown in Figure 9.17:

![](img/d34207e07c133a9e84d9caf1d479006c_264_1.png)

Figure 9.17 – Setting min, max, and scale curve

This sets the scale from 0 to 1; however, I recommend experimenting with the curve values until you find a result that you are happy with.

The Smoke scene is complete and ready to be instanced in the Cannon script. When we instance a cannonball from within the Cannon script, we should instance the smoke just afterward. Now, let’s return to the Cannon script.

12. Add two new global variables called particles (to reference the particles scene file) and smoke (to be an instance of the scene):

```
extends Node3D
var ball_scene
var ball_instance
var marker
var particles
var smoke
```

13. Adapt the _ready function so that we also preload the smoke particles scene. The new lines are commented in the following code. Create an instance of the smoke, position it at the marker, make it a child of the Cannon scene, and set the emitting property to true:

```
func _ready():
    scene = preload("res://scenes/ball.tscn")
    particles = preload("res://scenes/smoke.tscn")
    ball = scene.instantiate()

    # Create an instance of the smoke particles
    smoke = particles.instantiate()

    marker = $Marker3D
    ball.position = marker.position

    # Also place the smoke effect at the Marker3D position
    smoke.position = marker.position

    add_child(ball)

    # Add the smoke effect to the current scene as a child
    add_child(smoke)
```

```gdscript
# Start the smoke particle effect so it emits immediately
smoke.emitting = true
```

14. Finally, we must create new instances in the timeout function so that a puff of smoke appears each time the cannon fires:

```gdscript
func _on_cannonball_timer_timeout():
    ball_instance = ball_scene.instantiate()
    smoke = particles.instantiate()
    smoke.position = marker.position
    ball_instance.position = marker.position
    add_child(ball_instance)
    add_child(smoke)
    smoke.emitting = true
```

With these adaptations to the script for the cannon, you can test the level again. You will notice now that each time a cannonball is fired, two little clouds of smoke trail into the air. This is a simple way to add some dynamism to the level.

Another way to make a level more engaging is to add audio, such as sound effects and background music. In the next section, we will learn how to work with audio.

## Adding audio to our level

It would be fun if the player character made a noise when it jumped. Let’s add some audio!

- 1. To do this, we need to record a sound ourselves or find one that is in the public domain with a Creative Commons license. Here’s one we can use for this project: https://opengameart.org/content/jump-sound-16bit.
- 2. Download the jump.wav file. In **FileSystem**, go to the assets folder, and create a subfolder called audio. Drop the sound file in there as shown in Figure 9.18:

![](img/d34207e07c133a9e84d9caf1d479006c_267_0.png)

Figure 9.18 – Adding the jump.wav file to our audio folder

3. Open the Player scene and add an AudioStreamPlayer node as a child. In the Stream property, drag and drop the audio file. Leave the rest of the properties unchanged. This is shown in Figure 9.19:

![](img/d34207e07c133a9e84d9caf1d479006c_267_1.png)

Figure 9.19 – Giving AudioStreamPlayer a sound file to play

4. Open the Player script and add a reference variable for AudioStreamPlayer to your list of onready variables:

```
@onready var model = $MeshInstance3D
@onready var jump_cooldown_timer = $Jump_cooldown_timer
@onready var audio_stream_player = $AudioStreamPlayer
```

5. We only want to play the jump sound effect when the player jumps. To do this, we tell AudioStreamPlayer to play the file when the jump function runs. Add that request as the first line in the jump function:

```
func jump():
    audio_stream_player.play()
```

6. That is all that is required to play a sound effect when the player jumps.

But what if we wanted to play an effect when the player lands?

1. Grab another CC0-licensed sound effect from Open Game Art (https://opengameart.org/content/jump-landing-sound) and add it to the audio folder.

2. To play the right sound at the right time, the code must be adjusted to first load the corresponding audio file into AudioStreamPlayer before attempting to play it.

3. Add another line as the first line in the jump function to first load the file before playing it:

```
func jump():
    audio_stream_player.stream =
    preload("res://assets/audio/jump.wav")
    audio_stream_player.play()
```

4. Similarly, when the player lands, we must load the corresponding audio file and then play it. Do so by adding these lines after scaling the player when it lands, in the handle_respawn() function:

```
if is_on_floor() and gravity > 2 and !previously_floored:
    model.scale = Vector3(1.25, 0.75, 1.25)
    audio_stream_player.stream =
    preload("res://assets/audio/jumpland.wav")
    audio_stream_player.play()
```

5. Using the technique in the code, we ensured that we loaded the correct audio file into the player before we played it. The next step is to add background music to the level. Start by downloading the background music (https://opengameart.org/content/background-music-2-the-ice-caves) and adding it to the audio folder.

6. This time, we will add an **AudioStreamPlayer** node to the **Level 1** scene. We will drag and drop the Background Music 2.ogg file into the **Stream** property. Set the **Autoplay** property to **On**. This is shown in Figure 9.20:

![](img/d34207e07c133a9e84d9caf1d479006c_269_0.png)

Figure 9.20 - Setting the Autoplay property of the Stream player

7. There is one more step, and that is to ensure that the track is looping. Expand the **Parameters** section and turn on looping as shown in Figure 9.21:

![](img/d34207e07c133a9e84d9caf1d479006c_269_1.png)

Figure 9.21 - Turning looping on for the background music

As shown in Figure 9.21, by turning on looping, we can have a background music track that plays repeatedly in the level.

In game design, repetitive movement is often used to create dynamic and lifelike effects, such as clouds drifting across the sky, waves lapping at the shore, or power-ups slowly rotating or hovering up and down. This type of movement is typically achieved by adjusting an object’s properties (such as position, size, or angle) back and forth in a smooth and natural way using the oscillations of a sine wave. In our final section, we will use the sine function to move our clouds slowly back and forth in our level.

## Using the sine function

As mentioned earlier, rather than having the clouds sitting still in the sky, we will drift them back and forth using a sine wave. Return to the Cloud scene and attach a script to the root node (Cloud).

The entire code to make the clouds drift is given here and then explained line by line afterward:

```gdscript
extends Node3D
@onready var mesh_instance_3d: MeshInstance3D = $MeshInstance3D
var time: float
func _process(delta):
    time += delta
    mesh_instance_3d.position =
    Vector3(get_sine(),get_sine(),get_sine())

func get_sine():
    return sin(time * 0.5) * 0.5
```

Let’s break down this code here for a better understanding:

- A variable called time is declared as a float to track the elapsed time. It is used to calculate the sine wave for movement.
- A reference is created for the StaticBody3D node of the cloud since this is essentially the cloud object that we will be moving.
- In the process function, we increase the time variable for each frame. This ensures that the sine wave animation progresses smoothly over time.
- Next, update the position property of the cloud (StaticBody3D) in every frame.
- The x,y, and z coordinates are set using the get_sine() function, which produces a smooth oscillating value.
- The get_sine() function calculates a smooth oscillation based on the time variable.
