no asteroids are spawned. Combined with the timeout it effectively stops
the current wave. From this state the controller will transition either to
GameOver if there are no lives left, or to PlacingSpaceship if there are.

+   SpawningEnemies: the normal state. This is the equivalent of the behavior
+   of the previous version without states. It transitions to StoppingWave when
+   the spaceship is destroyed.

+   PlacingSpaceship: the controller puts a Player object into play and sends
+   a GameEvent about it. This transitions automatically to waiting.

+   Waiting: similar to StoppingWave, this state is also time based but it always
+   transitions to SpawningEnemies. This state exists to allow the player some
+   time to relax after the new spaceship is placed on the screen.

![](bbox=[0.118, 0.331, 0.882, 0.584])

The GameController starts in the PlacingSpaceship state, which gets one life
and places a spaceship on the screen. The GameController will move into
waiting and then into SpawningEnemies.

When a SpaceshipHit event arrives we move into StoppingWave. Once there are
no more Asteroids on the screen, we check the number of lives remaining. If it
is 0, that's GameOver, otherwise, we go to PlacingSpaceship (which triggers a
LifeLost game event) and then moves to waiting again until we can move to
SpawningEnemies.

Let's see the code to start a game:

```
@Override
```