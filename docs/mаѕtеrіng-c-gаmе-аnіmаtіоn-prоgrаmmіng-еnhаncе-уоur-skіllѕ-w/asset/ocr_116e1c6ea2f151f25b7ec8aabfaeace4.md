We define a new enum class named cameraType in the Enums.h file:

```
enum class cameraType {
	free = 0,
	firstPerson,
	thirdPerson,
	stationary,
	stationaryFollowing
};
```

In the CameraSettings struct, a new variable of the cameraType type must be added:

```
cameraType csCamType = cameraType::free;
```

The names already tell a lot about the purpose of the camera types. Here is what all the types are about:

+   - The free camera is already known from previous code examples. We can freely move around the world in five degrees of freedom: three directions and two rotations. There are no movement restrictions for the camera or view.
+   - The firstPerson camera does what cameras in first-person games do: it allows you to see the virtual world through the eyes of the protagonist. In our application, we will attach the camera to the head of an instance and move both camera and view just as the instance does. However, there will be an option to unlock the view to avoid weird angles or motion sickness.
+   - The thirdPerson camera follows the selected instance like another person or a drone, showing the same angle of the virtual world as the instance sees but from the outside of the instance body. Some view adjustments are possible, like the distance from the instance, but both camera motion and view are controlled by the instance.
+   - The stationary camera can be compared to a fixed surveillance camera. The stationary camera can be placed anywhere in the world and at any angle, but once the camera is locked, no movement or view adjustment except the field of view is possible. The field of view setting is kept unlocked to allow zooming in and out, just like with a normal surveillance camera.
+   - Finally, the stationaryFollowing camera is a special type of surveillance camera. While both movement and view are not controllable by the user (again, with the exception of the field of view setting), this camera type will automatically center the selected instance, regardless of whether the instance is directly visible or not. And if the instance moves around, the camera will track that instance within the virtual world.