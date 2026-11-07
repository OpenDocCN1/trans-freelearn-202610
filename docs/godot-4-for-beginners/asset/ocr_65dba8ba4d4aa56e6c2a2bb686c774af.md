Recall that the cannon fires confetti as a small visual reward for the player when they complete the level and touch the checkpoint flag. It makes sense, then, that the cannon should be attached to the checkpoint. We can do this quite easily in Godot by dragging the confetti_cannon.tscn file into the scene tree of the CheckPoint scene, as shown in Figure 10.22:

![](bbox=[0.32, 0.18, 0.68, 0.7])

Figure 10.22 – Adding the confetti cannon to the CheckPoint scene

Once the confetti cannon is part of the CheckPoint scene, we will need to add some code to check_point.gd to call the fire method once the level is complete.