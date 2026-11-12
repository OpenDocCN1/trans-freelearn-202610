```
Import 'package:flutter/services.dart';
```

+   8. At the top of the update function and below our call to
+   super.update(dt) ;, let's change the speed value based on whether
+   the run button is pressed or whether the r key is pressed. Also, we will
+   create a Boolean to track whether we are moving using the keys if any
+   of our variables from step 5 are set to true:

```
speed = (hud.runButton.buttonPressed ||
keyRunningPressed) ? runningSpeed : walkingSpeed;
final bool isMovingByKeys = keyLeftPressed ||
keyRightPressed || keyUpPressed || keyDownPressed;
```

+   9. Below this, in the update function, we have a check – if the joystick is
+   non-zero, meaning that it is being used. This check looks like this:

```
if (!hud.joystick.delta.isZero()) {
```

Let's add an else if clause at the end of that if block for our key movement
and add the following code, which will go between the if block and the
else block:

```
} else if (isMovingByKeys) {
movePlayer(dt);
playing = true;
movingToTouchedLocation = false;
if (!isMoving) {
isMoving = true;
audioPlayerRunning = await
FlameAudio.loopLongAudio('sounds/running.wav',
volume: 1.0);
}

if (keyUpPressed && (keyLeftPressed ||
keyRightPressed)) {
animation = upAnimation;
currentDirection = Character.up;
} else if (keyDownPressed && (keyLeftPressed ||
keyRightPressed)) {
animation = downAnimation;
currentDirection = Character.down;
} else if (keyLeftPressed) {
```