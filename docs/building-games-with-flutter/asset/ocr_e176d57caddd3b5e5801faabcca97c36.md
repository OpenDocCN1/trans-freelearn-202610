```
animation = leftAnimation;
currentDirection = Character.left;
} else if (keyRightPressed) {
animation = rightAnimation;
currentDirection = Character.right;
} else if (keyUpPressed) {
animation = upAnimation;
currentDirection = Character.up;
} else if (keyDownPressed) {
animation = downAnimation;
currentDirection = Character.down;
} else {
animation = null;
}
```

This code works the same as the way the joystick handles movement, by trying to move our player if it doesn't collide with the water and playing the walking steps sound.

Then it sets the `animation` and `currentDirection` variables correctly based on the key pressed.

+   10. In the `stopAnimations` function, let's reset the movement keys when animations are stopped:

```
void stopAnimations() {
animation?.currentIndex = 0;
playing = false;
keyLeftPressed = false;
keyRightPressed = false;
keyUpPressed = false;
keyDownPressed = false;
}
```

If you run the game now on the Chrome device, you will be able to control George with the keys on the keyboard.

# Summary

In this chapter, we converted the game to work on the web by fixing user interface issues when resizing the browser window and allowing movement