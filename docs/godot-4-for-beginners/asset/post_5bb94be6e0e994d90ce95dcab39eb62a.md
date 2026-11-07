
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

- **Scene reload (respawning):** If the player’s position.y falls below -10 (meaning they’ve fallen off the level), we call get_tree().reload_current_scene() to reload the current scene and respawn the player at the starting position.
- **Resetting scale (restoring size):** We use model.scale.lerp(Vector3(1, 1, 1), delta * smoothing_factor) to gradually interpolate the player’s scale back to normal after any squash-and-stretch effects from jumping or landing.
- **Landing animation (squash effect):** If the player is currently on the ground (is_on_floor()), was falling fast (gravity > 2), and was not on the ground in the previous frame (!previously_floored), we trigger a squash effect by changing model.scale to Vector3(1.25, 0.75, 1.25).
- **Grounded state tracking:** Finally, we update the previously_floored variable to keep track of whether the player was on the ground during the last frame.

To keep all these mechanics running smoothly, we need to call our custom functions inside Godot’s main physics loop:

```
func _physics_process(delta):
    handle_controls(delta)
    handle_gravity(delta)
    handle_movement(delta)
    handle_rotation(delta)
    handle_respawn(delta)
```

The _physics_process(delta) function is Godot’s built-in loop for physics calculations, called every physics frame (by default, 60 times per second). It ensures our game logic runs consistently, regardless of frame rate. By calling our custom functions here, we keep gravity, movement, jumping, camera effects, and respawning in sync with the physics engine.

Now that our character controller is working – handling movement, jumping, and landing animations – it’s time to set up the camera so it follows the player correctly in 3D space. A well-configured camera improves the gameplay experience by helping players stay oriented and focused on their surroundings. In the next section, we’ll connect the camera to the player and script it to respond smoothly to player movement and rotation.

## Implementing a camera controller

To begin configuring the camera, we’ll first create a new scene that will act as the camera controller and then link it to the player.

- 1. Create a new scene and add a Node3D node as the root. Rename this to View. Now add a Camera3D node as a child. The scene tree is shown in Figure 8.20:

![](img/d34207e07c133a9e84d9caf1d479006c_239_0.png)

Figure 8.20 - Scene composition for View

- 2. Select the Camera3D node and, in Inspector, set the Current property to On – marking this camera as the one to use. You can also adjust the field of view by setting the FOV property to 40. This is shown in Figure 8.21:

![](img/d34207e07c133a9e84d9caf1d479006c_240_0.png)

Figure 8.21 – Setting the camera as current and adjusting FOV

- 3. Now, attach a script to the View node and save it as view.gd. Make sure that you set the template to Empty.

Now we are ready to start building our Camera Controller script, which will let the player move the camera and adjust the zoom using the keys on the keyboard, which we defined in the input map.

The camera needs a target node to look at or focus on. This will be our Player node. We will export this target as a variable shown in the following code:

```
@export_group("Properties")
@export var target: Node
```

This exported target variable will appear in Inspector, allowing us to drag and drop the Player node into it. The camera will then know what it should follow.

Now export variables to control the zoom levels and rotation speed of the camera:

```
@export_group("Zoom")
@export var zoom_minimum = 16
@export var zoom_maximum = 4
@export var zoom_speed = 10
```

These variables define the zoom behavior: zoom_minimum and zoom_maximum determine how far in and out the camera can zoom. Additionally, zoom_speed controls how quickly zooming happens when the input is triggered.

Next, let’s define the rotation_speed variable:

```
@export_group("Rotation")
@export var rotation_speed = 120
```

This variable sets how quickly the camera rotates around the player when the player rotates or when input is used to turn the camera.

We will use two standard variables to track the current rotation and zoom of the camera:

```
var camera_rotation: Vector3
var zoom = 10
```

Let’s explore this further:

- camera_rotation will store the current rotation angles (in degrees) of the camera
- zoom keeps track of the current zoom value, which we will modify over time based on player input

Finally, we need a reference to the actual Camera3D node within this scene so we can manipulate it:

```
@onready var camera = $Camera3D
```

Using @onready ensures that this variable is set once the node is added to the scene and ready to be used.

In the built-in _ready() function, we will store the initial rotation of the camera:

```
func _ready():
    camera_rotation = rotation_degrees # Initial rotation
```

This initializes the camera_rotation variable with the camera’s current rotation angles so we can update and modify it from a known starting point.

Much like the player, the camera can be moved around the scene. Let’s create a function to handle the camera input:

```
func handle_input(delta):
    # Rotation
    var input := Vector3.ZERO
    input.y = Input.get_axis("camera_left", "camera_right")
    input.x = Input.get_axis("camera_up", "camera_down")

    camera_rotation += input.limit_length(1.0) *
    rotation_speed * delta
    camera_rotation.x = clamp(camera_rotation.x, -80, -10)

    # Zooming
    zoom += Input.get_axis("zoom_in", "zoom_out") *
    zoom_speed * delta
    zoom = clamp(zoom, zoom_maximum, zoom_minimum)
```

The handle_input(delta) function handles camera rotation and zooming based on the input from the player. The horizontal and vertical rotation is stored in the variable called input based on the keys the user is pressing.

Next, the camera rotation is updated based on this input. The input is limited to a maximum of 1.0, which we multiply by the rotation_speed variable and delta to make everything smooth. Finally, we clamp the vertical rotation to values between -80 and -10 degrees. This is to avoid excessive tilting.

For zooming, we update the zoom variable based on the player input, which we then multiply by zoom speed and delta to ensure smooth zooming. Again, we limit the zoom level between our pre-defined minimum and maximum zoom values.

Next, the _physics_process function needs to be implemented as follows:

```
func _physics_process(delta):
    # Set position and rotation to targets
    self.position = self.position.lerp(target.position,
    delta * 4)
    rotation_degrees =
    rotation_degrees.lerp(camera_rotation, delta * 6)

    camera.position = camera.position.lerp(Vector3(0, 0,
    zoom), 8 * delta)

    handle_input(delta)
```

Linear interpolation is used again to smoothly move the camera’s position towards the target (the player).

We multiply by delta * 4 to control the speed of repositioning.

Now the current rotation is moved smoothly towards the desired rotation using linear interpolation and multiplying delta * 6 for the speed.

To make a smooth zooming effect, we move the camera’s position along the z axis to match the zoom level. We multiply by delta * 8 to control the speed.

Finally, we call the handle_input(delta) function, which handles the player’s input and controls the movements of the camera.

Although we hope that everything will function as expected, we need to test it, correct it, and test it again. This makes game development an iterative process in which we make a change, test to see that the change produces the result that we are expecting and continue in this manner. Therefore, our next step is to test what we have done so far.
