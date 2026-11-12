```
void movePlayer(double delta) {
if (!(hasCollided && collisionDirection ==
currentDirection)) {
if (movingToTouchedLocation) {
position.add((targetLocation -
position).normalized() * (speed * delta));
} else {
switch (currentDirection) {
case Character.left:
position.add(Vector2(delta * -speed, 0));
break;
case Character.right:
position.add(Vector2(delta * speed, 0));
break;
case Character.up:
position.add(Vector2(0, delta * -speed));
break;
case Character.down:
position.add(Vector2(0, delta * speed));
break;
}
}
}
}
```

15. Let's update the `update` function and replace the two places where we change the sprite position for both touch and joystick control.

Replace the following line with `movePlayer(dt); :`

```
position.add(hud.joystick.relativeDelta * speed * dt);
```

Also, replace the following line with `movePlayer(dt); :`

```
position += (targetLocation - position).normalized() *
(speed * dt);
```

If you run the app now and move around with the joystick or by touch, you will see that you can't go outside of the screen, as we now collide with the water and prevent the player from moving over the water and outside the bounds of the screen.