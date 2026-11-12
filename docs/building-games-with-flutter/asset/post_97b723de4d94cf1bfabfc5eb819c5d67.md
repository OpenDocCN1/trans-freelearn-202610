
If you run the game now, you will see the new components displayed with the existing game sprites:

Score: 0

![](img/a63c3fccbe888cd4d04006b5e1b6c273_86_0.png)

Figure 5.1 – The game with the HUD controls

In this section, we learned how to add a joystick, a button, and a score to our game and positioned them on the screen.

In the next section, we will connect the joystick to George's movement and make him run faster when the run button is pressed. We will also update the score by 10 when George collides with an enemy.

## Moving our character with onscreen controls

In this section, we will start by connecting George's movement to the joystick.

Currently, the `George` class inherits his movement from the base class, `Character`, which it shares with the `Skeleton` and `Zombie` classes. As George will have different movement code from the enemy sprites, let's refactor the code to allow the enemy sprites' movement code. We will move the existing movement code into an `EnemyCharacter` class, which will become the new base class for the enemy sprites and remove this code from the `Character` class.

Let's get started:

- 1. In the `components` folder, create a file called `character_enemy.dart`.
- 2. Open the file and add the code at https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter05/lib/components/character_enemy.dart.

In this code, the `EnemyCharacter` class extends our `Character` class, and we have copied the `onCollision`, `update`, and `changeDirection` functions straight from the `Character` class.

- 3. In the `changeDirection` function in the `EnemyCharacter` class, you will need to update the `switch case` block to prepend the directions with the Character base class. Once done, the block will look like this:

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

- 4. Open the zombie.dart and skeleton.dart files, and inside them both, instead of extending from Character, change the class definition to extend from EnemyCharacter.
- 5. Remove the previous import for character.dart and replace this with the following:

```
import 'character_enemy.dart';
```

- 6. Open the character.dart class and remove the functions for onCollision, update, and changeDirection that we now have in the EnemyCharacter class. You can also remove the import for the math package now too.
- 7. Open the file for george.dart and remove the line for changeDirection in the onLoad function.
- 8. If you run the game now, you will see George is invisible because we have not updated him and set his animation yet. Let's fix that by adding the following code in the onLoad function in the george.dart file:

```
animation = downAnimation;
playing = false;
```

- 9. Next, we will set up some variables for `walkingSpeed` and `runningSpeed` that will be toggled when the run button is pressed. At the top of the `George` class, below the constructor, add the following variables:

```
late double walkingSpeed, runningSpeed;
```

- 10. Then, at the top of the `onLoad` function, let's set `walkingSpeed` to equal the default speed that the sprite will move at and `runningSpeed` to be twice as fast:

```
walkingSpeed = speed;
runningSpeed = speed * 2;
```

- 11. Because we want to read the status on the joystick and the run button, we need to get a reference to `HudComponent`. So, let's pass the HUD via the constructor and store the HUD in a variable:

```
final HudComponent hud;
```

- 12. Then, update the constructor to pass the HUD so that we can access the values from the HUD in this class:

```
George({required this.hud, required Vector2 position,
required Vector2 size, required double speed}) :
super(position: position, size: size, speed: speed);
```

- 13. Next, we need to add the import for `HudComponent`:

```
import 'package:goldrush/components/hud/hud.dart';
```

- 14. Now, we will add the `update` function below the `onCollision` function and connect the run button to the `speed` variable:

```
@override
void update(double dt) {
super.update(dt);
speed = hud.runButton.buttonPressed ?
runningSpeed : walkingSpeed;
}
```

If the run button is pressed, the speed will be `runningSpeed`; otherwise, it will be `walkingSpeed`.

- 15. After the line that sets the speed for walking and running, let's add the code that links the joystick to the character's movements and set the character's direction and animation based on the direction of the joystick:

```
if (!hud.joystick.delta.isZero()) {
position.add(hud.joystick.relativeDelta * speed
* dt);
playing = true;

switch (hud.joystick.direction) {
case JoystickDirection.up:
case JoystickDirection.upRight:
case JoystickDirection.upLeft:
animation = upAnimation;
currentDirection = Character.up;
break;
case JoystickDirection.down:
case JoystickDirection.downRight:
case JoystickDirection.downLeft:
animation = downAnimation;
currentDirection = Character.down;
break;
case JoystickDirection.left:
animation = leftAnimation;
currentDirection = Character.left;
break;
case JoystickDirection.right:
animation = rightAnimation;
currentDirection = Character.right;
break;
case JoystickDirection.idle:
animation = null;
break;
}
} else {
if (playing) {
stopAnimations();
}
}
```

Here, if the joystick reading is 0, the character doesn't move, but if it is above 0, the character is moving. If the joystick is moving, we update our position based on the value from the joystick, the current speed, and `deltaTime`. Then, based on the joystick direction, we set the correct direction and animation. If there is no joystick movement, we check whether the animation is playing: if it is, we will call `stopAnimations`, which we will define next.

- 16. At the bottom of the class, we will create a new function called `stopAnimations` with the following code:

```
void stopAnimations() {
 animation?.currentIndex = 0;
 playing = false;
}
```

The `stopAnimations` function stops the animation from playing by setting the variable to `false` and sets the current animation index to `0`; this draws the first frame of the animation as a static image. This gives the impression that we animate the character while the joystick is being used and the character George stands still when we stop using the joystick.

- 17. Let's increase the score by 10 every time an enemy is killed. In the `onCollision` function, update the function like so to increase the score:

```
if (other is Zombie || other is Skeleton) {
 other.removeFromParent();
 hud.scoreText.setScore(10);
}
```

- 18. Finally, open the `main.dart` file and update the `onLoad` function so that we create `HudComponent` as its own variable. This is used to store the HUD value that is passed to the `George` class as a parameter:

```
@override
Future<void> onLoad() async {
 var hud = HudComponent();
 add(Background());
 add (George(hud: hud, position: Vector2(200, 400),
size: Vector2(48.0, 48.0), speed: 40.0));
add (Zombie(position: Vector2(100, 200),
size: Vector2(32.0, 64.0), speed: 20.0));
add (Zombie(position: Vector2(300, 200),
size: Vector2(32.0, 64.0), speed: 20.0));
add (Skeleton(position: Vector2(100, 600),
size: Vector2(32.0, 64.0), speed: 60.0));
add (Skeleton(position: Vector2(300, 600),
size: Vector2(32.0, 64.0), speed: 60.0));
add(ScreenCollidable ());
add(hud);
}
```

If you run the app now, you will be able to control George with the joystick and chase down and kill the enemies much quicker when you collide with them. If you press and hold the run button while you are controlling the joystick, you will see George move at his running speed.

In this section, we added a HUD containing a joystick, run button, and score text, which we connected to the game to control George's movement and update the score when George kills an enemy.

In the next section, we will show you an alternative way to control George instead of using the joystick control, by detecting touch events on the screen and then moving George to that location.

## Moving our character with touch

Now that we have George moving with the joystick, let's look at an alternative method for moving our character via screen touch events, which is very popular in games. With mobile devices having very sensitive high-resolution screens nowadays, we can use touch events or gestures to move our character from its current location to the location on the screen that was tapped. Using trigonometry, we can calculate the angle between the origin and target locations and use the angle to move George in the correct direction, with the correct animation that matches the direction.

We are already detecting a tap event for the run button via the HasTappables mixin. So, to detect touches on the screen, we need to add the Tappable
