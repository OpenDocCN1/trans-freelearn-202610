# Practical sessions

Here are some additions you could make to the code:

+   - Add a more complete gravity.
+   
+     Instead of just manipulating the vertical position of the instances, use a more complex way to apply gravity by using vertical acceleration. Since gravity is used in two places, in the instance for the speed and in the renderer for the collision detection, synchronization between both code parts is needed.
+   - Add collision detection for the third-person camera.
+   
+     In real games, the camera is also a normal object in the virtual world. And instead of clipping through the level data, the camera always stays inside the same boundaries as the character it follows, connected by a spring arm structure to allow adjustments if the camera is unable to keep the exact position behind the character. You can try to add a bounding box for the third-person camera and check the boundaries against the level octree too. Remember to also check and change the distance and height of the camera if a collision occurs.
+   - Enhance stair and cliff detection.
+   
+     To enable the instances climbing stairs, basic logic has been added to the code that compares the step height of stairs with the instance position. This idea may not work for different kinds of game maps, plus the instance may fall off a cliff when the stair detection returns the wrong result. Enhance the logic to have better stair detection and also prevent the instance from falling down when it “thinks” it is standing in front of a step when it is actually on the edge of an abyss.
+   - Align the instance’s feet to follow the slope and normal if the floor.
+   
+     Right now, the feet of the instances may still intersect the ground, depending on the slope of the ground and the instance position. Add some code, and maybe even more nodes to the model(s), to align the feet of the instance to the ground triangle. This adjustment should work for forward/backward inclination and for a sideway rotation of the feet to match the ground.