with the Character base class. Once done, the block will look like this:

```
switch (newDirection) {
  case Character.down:
    animation = downAnimation;
    break;
  case Character.left:
    animation = leftAnimation;
    break;
  case Character.up:
    animation = upAnimation;
    break;
  case Character.right:
    animation = rightAnimation;
    break;
}
```

+   4. Open the zombie.dart and skeleton.dart files, and inside them both, instead of extending from Character, change the class definition to extend from EnemyCharacter.

+   5. Remove the previous import for character.dart and replace this with the following:

```
import 'character_enemy.dart';
```

+   6. Open the character.dart class and remove the functions for onCollision, update, and changeDirection that we now have in the EnemyCharacter class. You can also remove the import for the math package now too.

+   7. Open the file for george.dart and remove the line for changeDirection in the onLoad function.

+   8. If you run the game now, you will see George is invisible because we have not updated him and set his animation yet. Let's fix that by adding the following code in the onLoad function in the george.dart file:

```
animation = downAnimation;
playing = false;
```