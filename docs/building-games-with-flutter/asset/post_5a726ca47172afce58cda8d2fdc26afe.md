
Character.down;
} else if (angle > 135 && angle < 225) {
  // Facing left
  isEnemyFacingPlayer = currentDirection ==
    Character.left;
} else if (angle > 225 && angle < 315) {
  // Facing up
  isEnemyFacingPlayer = currentDirection ==
    Character.up;
}
return isPlayerNear && isEnemyFacingPlayer;
}

In the `isPlayerNearAndVisible` function, we first measure the distance to the player from our enemy position, check whether it is less than our `DISTANCE_TO_TRACK` value, and then set the `isPlayerNear` value to `true` if needed.

Next, we use the `getAngle` function to get the angle between the enemy and player and then use this to check whether the angle we are facing matches the `currentDirection` we are facing. If this matches, then the enemy is facing the player and we set the `isEnemyFacingPlayer` flag as needed.

If both values are `true`, we will return `true` from this function to indicate the enemy is near enough and can see the player, which we will use in the `update` function next to change the `enemyMovementType` from walking to chasing.

- 6. Let's rewrite the `update` function in the `EnemyCharacter` class. First, remove the existing `update` function and replace it with the code from GitHub at https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter10/lib/components/character_enemy.dart.

Let's go through the changes we have made to this function.

The previous `update` code is now in a `switch/case` block if the enemy is in the default walking state. We initially call the `isPlayerNearAndVisible` function we created in `Step 4` and set our current speed to `chasingSpeed` if the player is near and visible, and to `walkingSpeed` if not.

If the `enemyMovementType` is `WALKING`, the enemy will walk around as before, but when `CHASING`, the enemy will run directly toward the player at the increased speed.

- 7. Now, let's update the `Skeleton` and `Zombie` classes to take a reference to the player class, `George`, which extends from the `Character` class, and pass this reference to the `EnemyCharacter` base class.

Open the `skeleton.dart` file and change the constructor like this:

```
Skeleton({required Character player, required Vector2 position, required Vector2 size, required double speed}) : super(player: player, position: position, size: size, speed: speed) {
```

- 8. Open the `zombie.dart` file and change the constructor like this:

```
Zombie({required Character player, required Vector2 position, required Vector2 size, required double speed}) : super(player: player, position: position, size: size, speed: speed) {
```

- 9. Add the following import to resolve the reference to `Character` in the constructor:

```
import 'package:goldrush/components/character.dart';
```

- 10. Let's now tie all this together by passing the player reference to the enemy classes.

Open the `main.dart` file and change the code where you add the enemies like this:

```
if (index % 2 == 0) {
add(Skeleton(player: george, position:
Vector2(position.x + gameScreenBounds.left,
position.y + gameScreenBounds.top), size:
Vector2(32.0, 64.0), speed: 20.0));
} else {
add (Zombie(player: george, position:
Vector2(position.x + gameScreenBounds.left,
position.y + gameScreenBounds.top), size:
Vector2(32.0, 64.0), speed: 20.0));
}
```

- 11. Also, because we changed the George's image earlier, we need to update the new size to 32, 32, where we create George in the onLoad function of main.dart:

```
var george = George(barrierOffsets: barrierOffsets,
hud: hud, position: Vector2(gameScreenBounds.left +
300, gameScreenBounds.top + 300), size:
Vector2(32.0, 32.0), speed: 40.0));
```

If you run the game now and move George near to an enemy while the enemy is facing George, you will see the enemy chase George. If the enemy catches George, you will see the enemy collide and explode, as discussed previously in the Animating with particles section of Chapter 9, Implementing Advanced Graphics Effects.

Let's change this now to give George a health value of 100%, which we will reduce by 25% every time an enemy attacks George and not increase our score. In the next chapter, we will add some user interface screens that will show Game Over when George's health reaches 0, but for now, we will just get the mechanism working.

- 12. In the hud folder, create a new file called health_text.dart and add the code from here: https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter10/lib/components/hud/health_text.dart.

This code block will look very familiar as it's the same as the ScoreText component, but with all the references to score changed to health.

- 13. Open the hud.dart file and add the following import:

```
import 'package:goldrush/components/hud/health_text.dart';
```

- 14. At the top of the `HudComponent` class, add the following variable to show the `HealthText` value:

```
late HealthText healthText;
```

- 15. In the `onGameResize` function, at the bottom of the `if` block, change the code as follows to initialize the `healthText` value and add it to the HUD:

```
scoreText = ScoreText(position: Vector2(
 gameScreenBounds.left + 80, gameScreenBounds.top +
 60));
healthText = HealthText(position: Vector2(
 gameScreenBounds.right - 80, gameScreenBounds.top +
 60));
add(joystick);
add(runButton);
add(scoreText);
add(healthText);
```

- 16. At the bottom of the `else` block in the same function, add the following line to update the `healthText` position:

```
joystick.position = Vector2(gameScreenBounds.left +
 80, gameScreenBounds.bottom - 80);
runButton.position = Vector2(gameScreenBounds.right -
 80, gameScreenBounds.bottom - 80);
scoreText.position = Vector2(gameScreenBounds.left +
 80, gameScreenBounds.top + 60);
healthText.position = Vector2(gameScreenBounds.right -
 80, gameScreenBounds.top + 60);
```

- 17. Open the `george.dart` file and, in the variables section at the top, add the following code:

```
int health = 100;
```

- 18. In the `onCollision` function, let's change the `if` block to check whether we have hit a `Zombie` or `Skeleton`, so the code looks like the following:

```
if (other is Zombie || other is Skeleton) {
  gameRef.add(ParticleComponent(explodingParticle(
    other.position, Colors.red)));
  other.removeFromParent();
  if (health > 0) {
    health -= 25;
    hud.healthText.setHealth(health);
  } else {
    // TODO: Show game over screen here
  }
  FlameAudio.play('sounds/enemy_dies.wav', volume:
    1.0);
}
```

If you run the code now, you will see the health value in the top-right corner, which will reduce every time you collide with an enemy, as shown in the following figure:

![](img/a63c3fccbe888cd4d04006b5e1b6c273_181_0.png)

## Figure 10.1 – Health score reduced when hit by an enemy

In the next section, we will add some water obstacles to the map and discuss how to navigate around them. We will also discuss how we can enhance our enemies' AI, meaning that they will not chase you if they can't see you because they are blocked by a water obstacle.

## Navigating obstacles with pathfinding

In this section, we will discuss how to move our character from A to B when there are obstacles in the way. There are many solutions to this problem, but a common solution in games development that we are going to use is called the A Star algorithm.

The algorithm is an efficient way to calculate a route on a 2D grid. Remember, a tile map is a 2D grid that uses tile IDs to represent what is drawn on the map. We provide the algorithm with our grid coordinates for our start location (the current location of George) and our end location (where we tap on the screen), along with a list of grid coordinates for any obstacles that are in the way. The algorithm then returns a list of grid offsets that represent a path, which we can follow to navigate to our touched location while avoiding all obstacles!

Our initial challenge is that when we move George around, we are using pixels to represent the location, but the algorithm works in grid coordinates. For instance, if George was at the top left of the map (ignoring the water), his pixel coordinate might be 48, 48, but his grid coordinate will be 1, 1.

Let's start by creating a new file with some helper functions that let us convert between George's world coordinate (pixel) and his grid coordinate:

- 1. First, create and open a new file in the utils folder called map_utils.dart, and then add the following code:

```
import 'package:flame/components.dart';
import 'package:flutter/material.dart';
const int TILE_SIZE = 32;
Offset worldToGridOffset(Vector2 mapLocation) {
    double x =
    (mapLocation.x / TILE_SIZE).floor().toDouble();
    double y =
    (mapLocation.y / TILE_SIZE).floor().toDouble();
    return Offset(x, y);
}
```
