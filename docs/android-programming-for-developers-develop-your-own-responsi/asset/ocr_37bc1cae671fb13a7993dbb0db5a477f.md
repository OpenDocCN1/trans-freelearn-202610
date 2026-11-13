```
sp.play(loseLifeID, 1, 1, 0, 0, 1);

if(mLives == 0){
    mPaused = true;
    setupAndRestart();
}
}
```

The next code uses the top of the ball and compares it to zero to see if it has reached the top of the screen. If it has, it just reverses the ball on the y axis, clears any potential obstacles, and plays a beep.

Add the highlighted code we have just discussed to the update method:

```
// Bounce the mBall back when it hits the top of screen
if(mBall.getRect().top < 0){
    mBall.reverseYVelocity();
    mBall.clearObstacleY(12);

    sp.play(beep2ID, 1, 1, 0, 0, 1);
}
```

The next code uses the left of the ball and compares it to zero to see if it has reached the left of the screen. If it has, it just reverses the ball on the x axis, clears any potential obstacles, and plays a beep.

Add the highlighted code we have just discussed to the update method:

```
// If the mBall hits left wall bounce
if(mBall.getRect().left < 0){
    mBall.reverseXVelocity();
    mBall.clearObstacleX(2);

    sp.play(beep3ID, 1, 1, 0, 0, 1);
}
```

The next code uses the right of the ball and compares it to mScreenX to see if it has reached the right of the screen. If it has, it just reverses the ball on the x axis, clears any potential obstacles, and plays a beep.

Add the highlighted code we have just discussed to the update method:

```
// If the mBall hits right wall bounce
if(mBall.getRect().right > mScreenX){
    mBall.reverseXVelocity();
```