A rotation also affects the reference coordinate system of the object.

Once the sprite knows how to handle rotation, we just need to initialize the rotation of the asteroid to a value that makes sense and is different for each asteroid.

First, we initialize the rotation to a random angle, so each asteroid has a different value. We add this line to the init method of the Asteroid:

```
mRotation = gameEngine.mRandom.nextInt(360);
```

Then we set the rotation speed proportional to the angle of the linear speed, so each asteroid has a rotation speed based on the inclination of the trajectory. This also goes inside the init method:

```
mRotationSpeed = angle*(180d Math.PI)250d;
```

We make the asteroid perform a rotation equivalent to the inclination angle for times each second.

Once the values have been initialized, we need to update them during the onUpdate call. We will add the following code to onUpdate:

```
mRotation += mRotationSpeed * elapsedMillis;
if (mRotation > 360) {
mRotation = 0;
}
else if (mRotation < 0) {
mRotation = 360;
}
```

We update the rotation and ensure that it is under the values that are valid [0-360].

# Note

The rotation of a transformation matrix is provided in degrees.

Note that the transformation matrix uses the angles in degrees and not in radians, while the mathematical operators we used for the speed expect it in radians. Always double-check in which units the angles are expected.