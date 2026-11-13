# # Our approach

It does not make sense to have a deterministic GameController for YASS, so we will go for the procedural generation.

We are going to make asteroids fall from the top of the screen with an angle that varies in the range of [-30,30] degrees. We will restrict their position on the x axis to be in the 50 percent central area of the screen.

The speed is going to be constant and they will spawn at a given interval, also a constant.

The code of the GameController for handling the generation of Asteroids is as follows:

```
@Override
public void onUpdate(long elapsedMillis, GameEngine gameEngine) {
mCurrentMillis += elapsedMillis;
long waveTimestamp = mEnemiesSpawned*TIME_BETWEEN_ENEMIES;
if (mCurrentMillis > waveTimestamp) {
// Spawn a new enemy
Asteroid a = mAsteroidPool.remove(0);
a.init(gameEngine);
gameEngine.addGameObject(a);
mEnemiesSpawned++;
}
}
```

We count the time the game has been running, then we calculate the time for the next enemy to spawn. This is the number of enemies we already created multiplied by the time between enemies.

If the current time of the game is greater than the time when the next enemy has to appear, then we spawn one and add it to the GameEngine.

To do so, we get an Asteroid from the pool of objects, just as we did with the bullets. We initialize it and add it to the GameEngine. Then, we add one to the number of enemies spawned.

The code spawns a new enemy, separating them by the number of milliseconds stated by TIME_BETWEEN_ENEMIES. We have set this at 500 milliseconds.