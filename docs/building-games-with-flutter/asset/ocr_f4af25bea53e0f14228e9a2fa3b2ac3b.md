```
Vector2(position.x + gameScreenBounds.left,
position.y + gameScreenBounds.top), size:
Vector2(32.0, 64.0), speed: 20.0));
}
```

+   11. Also, because we changed the George's image earlier, we need to update the new size to 32, 32, where we create George in the onLoad function of main.dart:

```
var george = George(barrierOffsets: barrierOffsets,
hud: hud, position: Vector2(gameScreenBounds.left +
300, gameScreenBounds.top + 300), size:
Vector2(32.0, 32.0), speed: 40.0));
```

If you run the game now and move George near to an enemy while the enemy is facing George, you will see the enemy chase George. If the enemy catches George, you will see the enemy collide and explode, as discussed previously in the Animating with particles section of Chapter 9, Implementing Advanced Graphics Effects.

Let's change this now to give George a health value of 100%, which we will reduce by 25% every time an enemy attacks George and not increase our score. In the next chapter, we will add some user interface screens that will show Game Over when George's health reaches 0, but for now, we will just get the mechanism working.

+   12. In the hud folder, create a new file called health_text.dart and add the code from here: https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter10/lib/components/hud/health_text.dart.

This code block will look very familiar as it's the same as the ScoreText component, but with all the references to score changed to health.

+   13. Open the hud.dart file and add the following import:

```
import
'package:goldrush/components/hud/health_text.dart';
```