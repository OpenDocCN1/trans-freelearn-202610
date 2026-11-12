```
@override
void update(double dt) async {
    super.update(dt);
    speed = (hud.runButton.buttonPressed ||
    keyRunningPressed ? runningSpeed : walkingSpeed;
    final bool isMovingByKeys = keyLeftPressed ||
    keyRightPressed || keyUpPressed || keyDownPressed;
    if (!hud.joystick.delta.isZero()) {
    moveByJoystick(dt);
    } else if (isMovingByKeys) {
    moveByKeyboard(dt);
    } else {
    if (movingToTouchedLocation) {
    moveByTouch(dt);
    } else {
    if (playing) {
    stopAnimations();
    }
    if (isMoving) {
    isMoving = false;
    audioPlayerRunning.stop();
    }
    }
}
}
```

Now that we have finished updating the `George` class, let's move on to the main class to connect everything together.

+   12. Open the `main.dart` file and add the following import for the map utils functions:

```
import 'package:goldrush/utils/map_utils.dart';
```

+   13. In the `onLoad` function, below where we load and play the background music, add the following code to initialize the water barriers and then pass these to the `George` class via the constructor for use with pathfinding:

```
FlameAudio.bgm.initialize();
await FlameAudio.bgm.load('music/music.mp3');
await FlameAudio.bgm.play('music/music.mp3', volume: 0.1);
```