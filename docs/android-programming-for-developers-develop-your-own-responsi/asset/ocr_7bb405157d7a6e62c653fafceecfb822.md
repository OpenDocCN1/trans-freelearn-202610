unless you explicitly save and restore information, the fragments will not remember the previous state.

# Note

The sensorLandscape and sensorPortrait modes do not destroy activities on rotation.

Some good news here: while using sensorLandscape, the rotation does not kill the Activity, so no extra work is required. This happens because the layout is exactly the same and nothing needs to be recreated.

If you plan to make a game that can rotate, you must pay extra attention to saving and restoring the status of the game when the orientation changes. This in itself is another good reason to keep the game locked to a particular orientation, be it landscape or portrait.

# Dealing with aspect ratios

Android devices come in a lot of different aspect ratios, form 4:3 to 16:9 at least. This is not counting the number of pixels.

While designing a game for multiple aspect ratios, there are basically two ways of doing it. For each of them, we design for the most extreme aspect ratio. We will be using the extra space for "smart letterboxes," which means that we can have more game view.