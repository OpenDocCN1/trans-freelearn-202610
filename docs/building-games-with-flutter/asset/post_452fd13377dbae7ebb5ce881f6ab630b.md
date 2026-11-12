
In this new class, we take `TiledComponent` via the constructor and add it as a child component, so it will be positioned in the same position as this class. Then, in the `onGameResize` function, we are adjusting the position and fixing the size of this component to the map size, 1600 x 1600.

Moving on to the `Background` class, in this class, we want to remove the `position` and `size` settings – that happens currently in the `onLoad` function – by removing this function and then setting `position` and `size` in the `onGameResize` function.

- 2. Open the `background.dart` file, remove the `onLoad` function completely, and add the following code:

```
@override
void onGameResize(Vector2 canvasSize) {
  super.onGameResize(canvasSize);
  Rect gameScreenBounds =
      getGameScreenBounds(canvasSize);
  if (canvasSize.x > 1600) {
    double xAdjust = (canvasSize.x - 1600) / 2;
    position = Vector2(gameScreenBounds.left +
        xAdjust, gameScreenBounds.top);
  } else {
    position = Vector2(gameScreenBounds.left,
        gameScreenBounds.top);
  }
  size = Vector2(1600, 1600);
}
```

As with the `TileMapComponent` object we just added, this sets `position` and `size` based on the new screen size, after a resize.

## Important Note

Please note that `onGameResize` is always called when the component is first created and then again any time after that if the screen is resized. Therefore, we don't need to do anything on `onLoad` anymore.

- 3. Next, let's add the imports:

```
import 'package:goldrush/utils/math_utils.dart';
import 'dart:ui';
```

- 4. Finally for the `Background` class, we are going to change the constructor to increase the `priority` of the component to ensure that touch events are being picked up currently. Please change the constructor to the following:

```
Background(this.george) : super(priority: 20);
```

In the next section, we will discuss how to fix the HUD components.

## Fixing the HUD components

Now let's move on to fixing the HUD components.

Currently, in our HUD, we use the margins to adjust the joystick, run button, and score text locations based on the corners of the screen. There are some known issues at present with the Flame library when the game screen resizes that prevent this from working correctly.

So, we will rewrite part of the HUD to use position instead of margins, and then we can apply our usual calculations of getting the game screen bounds and adjusting the position of the HUD components when the screen resizes.

As in step 2 of the *Fixing the background and tile map* section, with the `Background` class, we are going to remove the `onLoad` functionality and do the resizing in the `onGameResize` function. We are also going to split this function in two. The first time `onGameResize` is called, we need to create the HUD components and add them as children adjusting their positions based on the screen size. Every time after that when `onGameResize` is called, we will just update the HUD components' new positions.

To keep track of this, we will create a variable called `isInitialised` to track whether we have set up the HUD components already and call the correct code based on that:

- 1. Open the `hud.dart` file and add the `isInitialised` variable at the top of the class:

```
bool isInitialised = false;
```

- 2. Remove the `onLoad` function and add the following code:

```
@override
void onGameResize(Vector2 canvasSize) {
 super.onGameResize(canvasSize);
 Rect gameScreenBounds =
 getGameScreenBounds(canvasSize);
 if(!isInitialised) {
 } else {
 }
}
```

- 3. In the first `if` code block that checks whether `isInitialised` is false, add the following code:

```
final joystickKnobPaint =
 BasicPalette.blue.withAlpha(200).paint();
 final joystickBackgroundPaint =
 BasicPalette.blue.withAlpha(100).paint();
 final buttonRunPaint =
 BasicPalette.red.withAlpha(200).paint();
 final buttonDownRunPaint =
 BasicPalette.red.withAlpha(100).paint();
 joystick = Joystick(
 knob: CircleComponent(radius: 20.0, paint:
 joystickKnobPaint),
 background: CircleComponent(radius: 40.0, paint:
 joystickBackgroundPaint),
 position: Vector2(gameScreenBounds.left + 100,
 gameScreenBounds.bottom - 80),
 );
 runButton = RunButton(
 button: CircleComponent(radius: 25.0, paint:
 buttonRunPaint),
 buttonDown: CircleComponent(radius: 25.0, paint:
 buttonDownRunPaint),
 position: Vector2(gameScreenBounds.right - 80,
 gameScreenBounds.bottom - 80),
 onPressed: () => {}
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

- 4. In the else block, please add the following code, which will update the position of components that we created in the if block:

```
joystick.position = Vector2(gameScreenBounds.left +
  80, gameScreenBounds.bottom - 80);
runButton.position = Vector2(gameScreenBounds.right -
  80, gameScreenBounds.bottom - 80);
scoreText.position = Vector2(gameScreenBounds.left +
  80, gameScreenBounds.top + 60);
```

- 5. And finally, for the HudComponent class, let's add the imports at the top of the file:

```
import 'package:goldrush/utils/math_utils.dart';
```

Next, let's fix the Joystick and ScoreText classes to use positions instead of margin. Note the RunButton class already had the position value in its constructor, so we don't need to update the RunButton class.

- 6. Open the joystick.dart file and change the constructor to the following:

```
Joystick({
  required PositionComponent knob,
  PositionComponent? background,
  Vector2? position
}) :
  super(knob: knob, background: background,
  position: position);
```

- 7. We can also now remove the unused import:

```
import 'package:flutter/material.dart';
```

- 8. Open the score_text.dart file and change the constructor to the following:

```
ScoreText({Vector2? position}) : super(position:
  position);
```

Now let's switch our focus to tying up all these component changes by making changes in our main.dart file to use the new game screen bounds.

- 9. Open the main.dart file and add the following imports at the top of the file:

```
import 'package:goldrush/components/tilemap.dart';
import 'package:goldrush/utils/math_utils.dart';
```

In the onLoad function, we want to set up the originalPosition variables for each component by calculating the game screen bounds and using the result from this along with our intended position to adjust the position when the screen is resized.

- 10. Continuing in the main.dart file, let's add the code to get the game screen bounds below where we call onLoad in the base class:

```
@override
Future<void> onLoad() async {
  super.onLoad();
  Rect gameScreenBounds =
  getGameScreenBounds(canvasSize);
```

- 11. Next, let's update where we create the `George` class by passing the `position`. Let's also change the `priority` of the `George` component to fix a loading issue that only happens on the web:

```
var george = George(hud: hud, position:
    Vector2(gameScreenBounds.left + 300,
        gameScreenBounds.top + 300), size: Vector2(48.0,
        48.0), speed: 40.0);
add(george);
children.changePriority(george, 15);
```

- 12. Where we create and load the `TiledComponent` data, we now need to wrap it with our new `TileMapComponent`:

```
final tiledMap = await
    TiledComponent.load('tiles.tmx', Vector2.all(32));
add(TileMapComponent(tiledMap));
```

- 13. Let's change the positions of the `Skeleton` and `Zombie` classes next:

```
if (index % 2 == 0) {
    add(Skeleton(position: Vector2(position.x +
        gameScreenBounds.left, position.y +
        gameScreenBounds.top), size: Vector2(32.0,
        64.0), speed: 60.0));
} else {
    add(Zombie(position: Vector2(position.x +
        gameScreenBounds.left, position.y +
        gameScreenBounds.top), size: Vector2(32.0,
        64.0), speed: 20.0));
}
```

- 14. Next, let's fix the `Coin` class with the new `position`:

```
double posCoinX = (randomX * 32) + 5 +
    gameScreenBounds.left;
double posCoinY = (randomY * 32) + 5 +
    gameScreenBounds.top;
```

- 15. And next, let's fix the `Water` class' `position`:
