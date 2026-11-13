# Updating the spaceship and bullets

Now that we have a Sprite base class, we have to update the existing Player and Bullet classes to extend from it.

Most of the member variables from the Player object are now a part of the Sprite. We can also remove the old implementation of onDraw and rely on the one from the Sprite.

Finally, we have new simpler constructors that just receive the GameEngine. The one for the Player class is:

```
public Player(GameEngine gameEngine) {
    super(gameEngine, R.drawable.ship);
    mSpeedFactor = mPixelFactor * 100d / 1000d;

    mMaxX = gameEngine.mWidth - mImageWidth;
    mMaxY = gameEngine.mHeight - mImageHeight;

    initBulletPool(gameEngine);
}
```

The one for the Bullet object is:

```
public Bullet(GameEngine gameEngine) {
    super(gameEngine, R.drawable.bullet);
    mSpeedFactor = gameEngine.mPixelFactor * -300d / 1000d;
}
```

All in all, we have pushed a lot of code to the Sprite class, which will make the inclusion of new game elements easier.

Before this, let's also add a frame-per-second counter to the GameView and see how it is performing to be able to compare StandardGameView and SurfaceGameView.