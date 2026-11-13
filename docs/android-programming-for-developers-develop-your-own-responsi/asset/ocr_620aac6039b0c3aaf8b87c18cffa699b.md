we want. Again, the order of the transformations affects the result.

While the scale is a part of the transformation matrix, the alpha is a parameter of Paint. Its value goes from 0 (transparent) to 255 (opaque). It is important that you remember to initialize it to 255 or all our sprites will be transparent.

# ## Note

Alpha values go from 0 (transparent) to 255 (opaque).

With regard to the time to live, we just need to check the total amount of time the particle has been active during onUpdate to remove it from the GameEngine and return it to the particle system when it has expired.

Particle is then a class that extends Sprite, which has a method to activate it. It runs its updates based on the values of linear and angular speed:

```
```java
public class Particle extends Sprite {
    private long mTimeToLive;
    private long mTotalMillis;

    public double mSpeedX;
    public double mSpeedY;

    public double mRotationSpeed;

    protected Particle(
        ParticleSystem particleSystem,
        GameEngine gameEngine,
        int drawableRes) {
        super(gameEngine, drawableRes, BodyType.None);
        mParent = particleSystem;
    }

    @Override
    public void removeFromGameEngine(GameEngine gameEngine) {
        super.removeFromGameEngine(gameEngine);
        mParent.returnToPool(this);
    }

    public void activate(
        GameEngine gameEngine,
        long timeToLive,
        double v
```
```