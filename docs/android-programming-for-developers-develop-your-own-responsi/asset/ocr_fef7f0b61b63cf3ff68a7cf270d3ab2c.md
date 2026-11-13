```
```java
// The SurfaceView class implements onTouchListener
// So we can override this method and detect screen touches.
@Override
public boolean onTouchEvent(MotionEvent motionEvent) {
    switch (motionEvent.getAction() & MotionEvent.ACTION_MASK) {
        // Player has touched the screen
        case MotionEvent.ACTION_DOWN:
            mPaused = false;

            // Is the touch on the right or left?
            if(motionEvent.getX() > mScreenX / 2){
                mPaddle.setMovementState(mPaddle.RIGHT);
            }else{
                mPaddle.setMovementState(mPaddle.LEFT);
            }

            break;
        // Player has removed finger from screen
        case MotionEvent.ACTION_UP:
            mPaddle.setMovementState(mPaddle.STOPPED);
            break;
    }
    return true;
}
```
```

If you are wondering why we set mPaused to false in the
MotionEvent.ACTION_DOWN case, it is because we pause the game when the
player runs out of lives. When the player taps the screen, this will then have the
effect of starting it again.

We are nearly there now.