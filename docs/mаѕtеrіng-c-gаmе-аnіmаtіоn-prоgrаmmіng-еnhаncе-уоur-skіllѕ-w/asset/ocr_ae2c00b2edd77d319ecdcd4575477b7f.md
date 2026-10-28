# Practical sessions

Here are some additions you could make to the code:

+   - Add a button to reload the music tracks from the asset folder.
+     A nice-to-have feature to dynamically add or remove tracks.
+   - Add more sound effects for the instances.
+     You could map a sound clip to every action of the instances, creating more realistic effects when controlling the selected instance.
+   - Play the step sound depending on the floor material.
+     Here, you need to load more than one pair of footstep sounds for walking and running. The appropriate sound effect could be chosen by using the mesh type of the ground triangle as the index into the footstep sounds.
+   - Add sounds for nearby instances.
+     The SDL_mixer library allows us to set the volume and balance per channel. Using the distance and angle of the surrounding instances can be used to calculate the effective volume and direction of the sounds.
+   - Play ambient sound at special places on the map.
+     Similar to the waypoint models, you could use small models as markers to play an ambient sound. And like the sounds for nearby instances, the volume and balance of the ambient sound can be adjusted to match the expectation.
+   - Add markers for music changes.
+     This task is closely related to the ambient sound. You could use models and/or nodes to switch the music when your controlled instance enters a specific area of the map or interacts with other instances.
+   - Adjust level color and fog depending on the level location.
+     Not only can the music be changed when roaming through the level; also, the light and fog settings can be adjusted. For instance, when entering a cave, the ambient light can be lowered and the fog density can be raised.