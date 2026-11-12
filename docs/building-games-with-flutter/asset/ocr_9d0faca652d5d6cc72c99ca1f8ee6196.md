+   1. Let's add some variables at the top of the `George` class to store the animations and direction information:

```
late SpriteAnimation georgeDownAnimation,
 georgeLeftAnimation, georgeUpAnimation,
 georgeRightAnimation;
 double elapsedTime = 0.0;
 double georgeSpeed = 40.0;
 int currentDirection = down;
 static const int down = 0, left = 1, up = 2,
 right = 3;
```

+   2. Below the `onLoad` function, define a new function called `changeDirection`, which we will call every 3 seconds to change George's direction randomly:

```
void changeDirection() {
 Random random = Random();
 int newDirection = random.nextInt(4);
 switch (newDirection) {
 case down:
 animation = georgeDownAnimation;
 break;
 case left:
 animation = georgeLeftAnimation;
 break;
 case up:
 animation = georgeUpAnimation;
 break;
 case right:
 animation = georgeRightAnimation;
 break;
 }
 currentDirection = newDirection;
}
```

Here, we generate a random number between 0 and 3 that maps to our columns. Then, we set the `animation` class field to use the animations, which we will initialize shortly. Finally, we set the current direction to be the new direction, which we can use to move the sprite later.

+   3. At the bottom of the `onLoad` function, remove the line that sets up the class field animation, as we now do this in the `changeDirection`