
Rect getGameScreenBounds(Vector2 canvasSize) {
}

- 2. At the top of the `getGameScreenBounds` function, add this code:

```
double left = 0, right = 0, top = 0, bottom = 0;
if (canvasSize.x > 1600) {
    left = (canvasSize.x - 1600) / 2;
}
if (canvasSize.y > 1600) {
    top = (canvasSize.y - 1600) / 2;
}
```

Here, we initially define some variables to store our `left`, `right`, `top`, and `bottom` values.

If the width or height of the screen is less than the map size, 1,600 x 1,600, we set the default values to `0`, `0` and only update the default values if the width or height of the screen is greater than the map size.

We pass in `canvasSize` to the function, which is the current screen size, and deduct `1600` to get the total difference between the two values. We then divide the total difference by `2` because we want to ensure any adjustments will center the map in the available space.

We will call this function at the start of the game, and every time the player changes the screen size by resizing the window, we will then adjust all the components based on this new screen size so they look correct.

- 3. Let's continue in our `getGameScreenBounds` function and set our `right` and `top` variables next:

```
if (canvasSize.x < 1600) {
    right = canvasSize.x;
} else {
    right = left + 1600;
}
if (canvasSize.y < 1600) {
    bottom = canvasSize.y;
} else {
    bottom = top + 1600;
}
return Rect.fromLTRB(left, top, right, bottom);
```

Here, we check whether the screen's width is less than 1600, and if it is, we set the right value to the screen's width. Otherwise, to get the right value, we add the left value and the map's width, 1600.

Then we do the same calculation for the bottom value using the screen's height instead.

Next, we create a rectangle to store our values and return it from the function.

## Important Note

All the values are based on the absolute pixel values to help us when we calculate the positions of the components, and especially the HUD, which must align closely with the corners.

- 4. Finally, we add the following import at the top of the file to resolve the references to the Vector2 and Rect classes we use in this function:

```
import 'package:flame/extensions.dart';
```

Now that we have the maths sorted for calculating the bounds when the screen resizes, let's start applying this to our components.

## Fixing the sprites

We will start with George and the enemies, which have a top-level base class of Character. So, let's listen for the game resize event in that class:

- 1. Open the character.dart file and add the following variable to the Character class to keep track of the original position of the sprite:

```
late Vector2 originalPosition;
```

- 2. Next, let's add the code to listen for the game resize event by overriding the onGameResize function that is part of the Component class, which all our sprites inherit from. Then we adjust the current position based on `originalPosition` and the current game screen bounds, as shown here:

```
@override
void onGameResize(Vector2 canvasSize) {
 super.onGameResize(canvasSize);
 Rect gameScreenBounds =
 getGameScreenBounds(canvasSize);
 position = Vector2(originalPosition.x +
 gameScreenBounds.left, originalPosition.y +
 gameScreenBounds.top);
}
```

- 3. Now that we have the code for handling the resizing, let's add the imports at the top of this file:

```
import 'package:goldrush/utils/math_utils.dart';
import 'package:flame/extensions.dart';
```

With the `Character` base class resizing now handled, it's very easy to fix George and the enemies. We just need to store the original position in the constructor and the sprites will move correctly when the screen is resized by their callback to `onGameResize`, which is handled in their `Character` base class.

- 4. So, open the `george.dart` file and change the constructor to this:

```
George({required this.hud, required Vector2 position,
 required Vector2 size, required double speed}) :
 super(position: position, size: size, speed:
 speed) {
 originalPosition = position;
}
```

- 5. Next, open the `skeleton.dart` file and change the constructor to this:

```
Skeleton({required Vector2 position, required Vector2
 size, required double speed}) : super(position:
 position, size: size, speed: speed) {
 originalPosition = position;
}
```

- 6. Then, open the `zombie.dart` file and change the constructor to this:

```
Zombie({required Vector2 position, required Vector2 size, required double speed}): super(position: position, size: size, speed: speed) {
 originalPosition = position;
}
```

Now that we have fixed our main sprites, let's fix the `Coin` and `Water` components.

### Fixing the coin and water components

To start fixing the water and coin components, perform the following steps:

- 1. Open the `coin.dart` file, change the `Coin` class constructor, and add a variable for the original position like this:

```
Coin({required Vector2 position, required Vector2 size}):
 originalPosition = position,
 super(position: position, size: size);
late Vector2 originalPosition;
```

- 2. Add the following `onGameResize` function to the `Coin` class:

```
@override
void onGameResize(Vector2 canvasSize) {
 super.onGameResize(canvasSize);
 Rect gameScreenBounds =
 getGameScreenBounds(canvasSize);
 position = Vector2(originalPosition.x +
 gameScreenBounds.left, originalPosition.y +
 gameScreenBounds.top);
}
```

- 3. And finally for the `Coin` class, let's add the imports at the top of the file:

```
import 'dart:ui';
import 'package:goldrush/utils/math_utils.dart';
```

The code we have added to the `Coin` class is similar to what we added to the `Character` classes where we store the `originalPosition` in the constructor, and then use this value with the new screen size when the game gets resized to calculate our new position.

Let's do the same for the `Water` class.

- 4. Open the `water.dart` file, change the `Water` class constructor, and add a variable for the original position like this:

```
Water({required Vector2 position, required Vector2 size, required this.id}) :
 originalPosition = position,
 super(position: position, size: size);
late Vector2 originalPosition;
```

- 5. Add the following `onGameResize` function to the `Water` class:

```
@override
void onGameResize(Vector2 canvasSize) {
 super.onGameResize(canvasSize);
 Rect gameScreenBounds =
 getGameScreenBounds(canvasSize);
 position = Vector2(originalPosition.x +
 gameScreenBounds.left, originalPosition.y +
 gameScreenBounds.top);
}
```

- 6. And finally for the `Water` class, let's add the imports at the top of the file:

```
import 'package:goldrush/utils/math_utils.dart';
import 'dart:ui';
```

Now that we have fixed the sprites, let's fix the background and the tile map.

## Fixing the background and tile map

For our tile map, we currently use `TiledComponent`, which itself extends from the `Component` class. This is the base class for all other components, and this class doesn't itself have a position. This is a problem for tracking the game bounds for the tile map. So, the solution for this is to wrap our `TiledComponent` around another class, which we will name `TileMapComponent`.

`TileMapComponent` is itself a position component and we will make `TiledComponent` a child of the new `TileMapComponent` class. By doing this, we can freely position this new class when the screen resizes.

So, let's continue and add this new wrapper class:

- 1. In the `components` folder, create a new file called `tilemap.dart` and add the following code:

```
import 'package:flame/components.dart';
import 'package:flame_tiled/flame_tiled.dart';
import 'package:flutter/material.dart';
import 'package:goldrush/utils/math_utils.dart';
class TileMapComponent extends PositionComponent {
  TileMapComponent(this.tiledComponent) {
    add(tiledComponent);
  }
  TiledComponent tiledComponent;
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
}
```
