# Handling collisions

Regardless of the method we use to calculate the collisions, the actions we have to take for the player and the bullets are the same. Let's override the method onCollision for them.

In the case of the bullets, we have to check if the object it collides with is an asteroid and, if that's the case, we remove both objects from the GameEngine.

```
public void onCollision(GameEngine gameEngine, ScreenGameObject
otherObject) {
if (otherObject instanceof Asteroid) {
// Remove both from the game (and return them to their pools)
removeObject(gameEngine);
Asteroid a = (Asteroid) otherObject;
a.removeObject(gameEngine);
}
}
```

Note that we are calling a method removeObject on the objects themselves. This method takes care of removing the GameObject from the GameEngine, and also returning it to the object pool.

The code for the player is almost identical to the one for the bullet: we just remove both colliding objects in the case of an asteroid.

It is worth mentioning that because of the way we are spawning the bullets, they do collide with the Player object when added to the scene because we want it to look like they appear from the spaceship. We have to discard that collision. It is good practice to always check what the object is colliding against.

If we want to make a game with several lives, we should signal the GameEngine at that point to stop spawning waves, remove one life, spawn a new Player object, and then continue the game.

Let's see how the collisions are actually calculated.