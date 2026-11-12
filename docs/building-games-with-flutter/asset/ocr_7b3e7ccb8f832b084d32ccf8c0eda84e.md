14. At the top of the `HudComponent` class, add the following variable to show the `HealthText` value:

```
late HealthText healthText;
```

15. In the `onGameResize` function, at the bottom of the `if` block, change the code as follows to initialize the `healthText` value and add it to the HUD:

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

16. At the bottom of the `else` block in the same function, add the following line to update the `healthText` position:

```
joystick.position = Vector2(gameScreenBounds.left +
 80, gameScreenBounds.bottom - 80);
RunButton.position = Vector2(gameScreenBounds.right -
 80, gameScreenBounds.bottom - 80);
scoreText.position = Vector2(gameScreenBounds.left +
 80, gameScreenBounds.top + 60);
healthText.position = Vector2(gameScreenBounds.right -
 80, gameScreenBounds.top + 60);
```

17. Open the `george.dart` file and, in the variables section at the top, add the following code:

```
int health = 100;
```

18. In the `onCollision` function, let's change the `if` block to check whether we have hit a `Zombie` or `Skeleton`, so the code looks like the following: