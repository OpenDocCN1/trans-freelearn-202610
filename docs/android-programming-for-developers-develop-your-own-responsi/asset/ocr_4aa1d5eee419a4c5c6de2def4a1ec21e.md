The explode method does trigger oneShot from the center of the asteroid, which will put all the particles in motion.

The last connection point is to trigger the explode method. This happens when a bullet collides with an asteroid or when the player collides with an asteroid. This is how it is done inside the Bullet class:

```
@Override
public void onCollision(GameEngine gameEngine, ScreenGameObject otherObject) {
    if (otherObject instanceof Asteroid) {
        removeFromGameEngine(gameEngine);
        Asteroid a = (Asteroid) otherObject;
        a.explode(gameEngine);
        a.removeFromGameEngine(gameEngine);
    }
}
```

This is it. Quite simple, right? Since the ParticleSystem is using a oneShot, it does not need to be added or removed to or from the GameEngine.

![](bbox=[0.112, 0.473, 0.882, 0.686])

Detail of the explosion particle and particle system.