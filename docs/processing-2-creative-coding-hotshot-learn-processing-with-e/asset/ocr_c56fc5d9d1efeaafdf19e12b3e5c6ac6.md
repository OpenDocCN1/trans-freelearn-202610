+   6. Run your sketch now. The rocket should start from the top of our window, drop to the floor, and stop there as shown in the following screenshot:

![](bbox=[0.371, 0.191, 0.63, 0.396])

+   7. Since the game is not about crashing rockets into the moon but rather about landing them, we need to implement a method to slow down the rocket. The game will be controlled using the keyboard, so let's add a variable for the acceleration and the rotation angle.

```
int[] moon;
int landingX = 0;
PImage ship;

PVector pos = new PVector( 150, 20 );
PVector speed = new PVector( 0, 0 );
PVector g = new PVector( 0, 1.622 );

float a = 0;
float acc = 0;
```

+   8. Also, add a keyPressed() method to enable the player to change the rotation and acceleration of the rocket.

```
void keyPressed() {
  if ( keyCode == LEFT ) {
    a -= 0.1;
  }

  if ( keyCode == RIGHT ) {
    a += 0.1;
  }

  if ( keyCode == UP ) {
    acc += 0.04;
  }
```