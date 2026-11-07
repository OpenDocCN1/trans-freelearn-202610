Let's define these variables in the script:

```
var previously_floored = false
var jump_single = true
var jump_double = true
var gems = 0
var smoothing_factor = 10

const GRAVITY = 25
```

In this code snippet, previously_floored tracks whether the character was on the ground during the last frame and is useful for detecting landings. This will become clearer when it is used in code further on.

Finally, we need a reference variable for the 3D mesh that visually represents the character. We'll use this reference to adjust the model's scale during jumping and landing – an easy way to add visual appeal known in animation as squash and stretch.

```
@onready var model = $MeshInstance3D
```

This line tells Godot to assign the MeshInstance3D node (a child of the current node) to the variable model, but only after the scene is fully loaded and ready. The @onready keyword ensures that the node exists when this reference is made.

# Alternative method for adding reference variables

There is a shortcut for adding reference variables such as the preceding one used for the player model. You drag the node that you want to reference into the script, and, as you are about to release the mouse button, hold Ctrl on the keyboard. Godot will automatically type out the reference for you, and you can rename it if you wish. This is shown in Figure 8.17:

![](bbox=[0.22, 0.6, 0.78, 0.78])

Figure 8.17 – Drag the node into the script and hold down Ctrl as you release it