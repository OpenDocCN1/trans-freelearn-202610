```
add(ScreenCollidable());
}
```

+   15. Also, let's import `package:flame/components.dart` where `ScreenCollidable` is, as follows:

```
import 'package:flame/components.dart';
```

+   16. Now, in our `Player` class, in the line that defines the class, we need to add the `HasHitboxes` and `Collidable` mixins:

```
class Player extends PositionComponent with
HasHitboxes, Collidable {
```

This will allow us to receive a callback when we collide with the sides of the screen in the `onCollision` function, which we will implement next.

+   17. Implement the `onCollision` function by adding the following:

```
@override
void onCollision(Set<Vector2> points,
    Collidable other) {
  if (other is ScreenCollidable) {
    if (squareDirection == 1) {
      squareDirection = -1;
    } else {
      squareDirection = 1;
    }
  }
}
```

Here, when we receive a callback to `onCollision` to tell us what component we have collided with, we check whether we have collided with `ScreenCollidable`, and if so, we simply flip the direction the square is traveling in.

+   18. Now that we are using the built-in collision tracking system, we must set up the position of our sprite correctly for the bounding box to be set, so we will use the built-in position variable that is inherited from the `PositionComponent` component.