
In all cases, the sensors are related to the default orientation of the device, which can be either landscape or portrait. We have to take this into consideration while processing the values.

All in all, the conversion for the horizontal axis can be done like this:

```
private double getHorizontalAxis() {
if (SensorManager.getRotationMatrix(mRotationMatrix, null,
mLastAccels, mLastMagFields)) {
if (mRotation == Surface.ROTATION_0) {
SensorManager.remapCoordinateSystem(mRotationMatrix,
SensorManager.AXIS_Y, SensorManager.AXIS_MINUS_X, mRotationMatrix);
SensorManager.getOrientation(mRotationMatrix, mOrientation);
return mOrientation[1] * DEGREES_PER_RADIAN;
}
else {
SensorManager.getOrientation(mRotationMatrix, mOrientation);
return -mOrientation[1] * DEGREES_PER_RADIAN;
}
}
else {
// Case for devices which do NOT have magnetic sensors
if (mRotation == Surface.ROTATION_0) {
return -mLastAccels[0] * 5;
}
else {
return -mLastAccels[1] * -5;
}
}
}
```

The getHorizontalAxis code does the following steps:

- Calculates the rotation matrix using the last data from the accelerometer and the magnetic sensor.
- If it returns true, all goes well. Based on the rotation of the device, we decide whether we need to remap the coordinate system or not and then return the orientation that is converted into degrees.
- If it could not be calculated (lack of a magnetic field sensor), the method returns false. We have to rely on an approximation using the accelerometer values. Based on the rotation of the device, we should use one or another axis.

The rotation of the device can be read in the constructor of the InputController with a single line of code.

```
mRotation =
yassActivity.getWindowManager().getDefaultDisplay().getRotation();
```

Finally, the onPreUpdate method:

```
@Override
public void onPreUpdate() {
    mHorizontalFactor = getHorizontalAxis() / MAX_ANGLE;
    if (mHorizontalFactor > 1) {
        mHorizontalFactor = 1;
    }
    else if (mHorizontalFactor < -1) {
        mHorizontalFactor = -1;
    }
    mVerticalFactor = 0;
}
```

This method just converts the reading (in degrees) into values in the range
[-1,1] by using the maximum angle at which we consider it to be fully tilted. I
recommend you to play with this constant and start with 30 degrees.

For more information on handling sensors, you can check the official
documentation

http://developer.android.com/guide/topics/sensors/sensors_overview.html.

## Selecting control modes

It is common for games to ask the user to select the control mode they prefer, but
it is also a good practice to avoid friction as much as possible by not asking what
is not necessary.

YASS is only using a virtual joystick and the gamepad controls. There is no
need to ask the user which one he or she wants. Both input modes are
compatible, especially because the virtual joystick does not display anything on
the screen when it is not used. The only thing we need to do is to modify the
GameEngine to support more than one InputController.

**Note**

We will support both input modes simultaneously.

The way to support both input modes at the same time is to create a
CompositeInputController that uses the composition pattern to have both a
VirtualJoystickInputController and a GamepadInputController, and
combines the input from both.

To synchronize the readings from the two input controllers, we are going to use
a method on the InputController called onPreUpdate, which will be called just
before onUpdate. We will use it to populate the values of mHorizontalFactor,
mVerticalFactor, and mIsFiring with the ones read from the other controllers.

```
public void onPreUpdate() {
    mIsFiring = mGamepadInputController.mIsFiring ||
    mVJoystickInputController.mIsFiring;
    mHorizontalFactor = mGamepadInputController.mHorizontalFactor +
    mVJoystickInputController.mHorizontalFactor;
    mVerticalFactor = mGamepadInputController.mVerticalFactor +
    mVJoystickInputController.mVerticalFactor;
}
```

We now have a game that can be controlled with a virtual joystick and a
gamepad.

## Summary

We have learned how to deal with input from users in several ways and how to make it transparent for the GameEngine.

To get a proper visual feedback from the controller, we created a Player game object that updates its position based on the values from the InputController. We also learned how to add and remove game objects to and from the GameEngine while playing.

We created a very basic keypad that later evolved into a virtual joystick. We also learned how to handle external controllers.

At this point, our game has a spaceship that moves along the screen and fires bullets. It can be controlled using a virtual joystick or a gamepad, independently.

The current implementation does lag occasionally and we have barely started drawing objects on the screen. It is time to fix this. Next stop: improve the rendering by drawing directly on the view instead of relying on positioning views on the screen.

## Chapter 3. Into the Draw Thread

In this chapter, we are going to improve the rendering of sprites for our game. For this, we are going to use a custom GameView that will perform low-level drawing. We will make two different implementations: one that extends from View and another one that extends from SurfaceView. We will let DrawThread be a real thread, to work better with this GameView.

We will refactor the project, creating a Sprite class that will be used for all the items that are drawn in the game. We will draw bitmaps on a Canvas and learn about the transformation matrix used to do this.

To continue improving the game, we will add enemies. They will be a wave of asteroids moving towards our spaceship. For this, we will learn the concept of GameController and the different ways of doing it, from static to procedural level generation.

As part of the rendering techniques, we will learn about occlusion culling and parallax backgrounds, which we will use to make the game look nicer.

Finally, we will add support for layers in the engine.

## Using GameView

Until now, we have been using standard views and translating them to render the different elements of the game. While this is an easy way to draw elements on the screen, it is far from being efficient. We are relying on the system layout to do the drawing.

While this technique is fine for a turn-based game or any non-real-time game in general, it cannot render enough frames per second for a real-time game.

> **Note**
> Working with standard Views is fine for non-real-time games.

We are going to create a custom view that we are going to call GameView. This view will be responsible for drawing the sprites.

We already noted the duplication of code and mentioned the concept of sprite in the previous chapter. We will now move forward and create a Sprite class that will take care of drawing an image at specific coordinates inside the GameView.

There are two ways of drawing at low level on Android. They are:

- Extending View and overriding onDraw
- Extending SurfaceView and using SurfaceHolder

In both cases, we will get a Canvas and draw our GameObjects on it. The main difference is that the onDraw method of the View is executed on the UIThread, while SurfaceView and SurfaceHolder are designed to perform the draw on a separate thread.

> **Note**
> Low-level drawing on Android is always done using a canvas.

According to the official documentation, it is more efficient to use SurfaceView. But, since Android 4.0, view rendering is hardware-accelerated (while SurfaceView is not). In the case of modern phones with high-resolution screens and faster processors, this may not always be the case.

> **Note**
> SurfaceView is not hardware-accelerated and may perform worse than normal View.

Anyway, you should know both and be able to swap them easily, even if it is just for testing purposes. We will create an interface named GameView, which will be implemented by the two classes, so they can be changed easily. The classes we will make are:

- StandardGameView: This will extend from View
- SurfaceGameView: This will extend from SurfaceView
