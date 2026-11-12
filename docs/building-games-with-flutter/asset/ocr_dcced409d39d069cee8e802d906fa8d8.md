```
);
scoreText = ScoreText(position: Vector2(
  gameScreenBounds.left + 80, gameScreenBounds.top +
  60));
add(joystick);
add(runButton);
add(scoreText);
positionType = PositionType.viewport;
isInitialised = true;
```

Most of this code will be familiar from our previous onLoad function. Then we set up our HUD components using position instead of margins and add them as children. And then set the isInitialised variable to true, so we don't rerun this code every time the game is resized and keep on adding more components.

You may notice at this point that the child components show an error as they don't currently have a position value, but we will fix that soon.

+   4. In the else block, please add the following code, which will update the position of components that we created in the if block:

```
joystick.position = Vector2(gameScreenBounds.left +
  80, gameScreenBounds.bottom - 80);
runButton.position = Vector2(gameScreenBounds.right -
  80, gameScreenBounds.bottom - 80);
scoreText.position = Vector2(gameScreenBounds.left +
  80, gameScreenBounds.top + 60);
```

+   5. And finally, for the HudComponent class, let's add the imports at the top of the file:

```
import 'package:goldrush/utils/math_utils.dart';
```

Next, let's fix the Joystick and ScoreText classes to use positions instead of margin. Note the RunButton class already had the position value in its constructor, so we don't need to update the RunButton class.

+   6. Open the joystick.dart file and change the constructor to the following: