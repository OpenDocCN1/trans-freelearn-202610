
## Summary

We have learned how to draw at low level using a standard view and also a SurfaceView. We have created a sprite class to reuse the code of the items that are displayed on the screen.

The DrawThread has been updated to a more efficient one and we also added a frames-per-second counter to check the efficiency of each configuration.

Along the way, we learned about game controllers and the different ways to create levels. We also decided to spawn asteroids YASS using procedural generation, and put this into effect.

We also added support for parallax backgrounds and layers to the engine.

All in all, YASS is starting to look good, but we are clearly missing something: the bullets do nothing when they hit an asteroid and neither does our spaceship.

It is time to implement collision detection.

## Chapter 4. Collision Detection

In most games we need to detect when objects intersect with each other to trigger actions; this is called collision detection. We will use it to detect when a bullet hits an asteroid (to destroy it) and when the player is hit by an asteroid (to end the game). This detection can be done in a discrete or in a continuous way, and it can involve different types of shapes. We will use discrete detection with rectangular and circular shapes.

As mentioned in Chapter 1, Setting Up the Project, we are not going to do any Physics simulation. That is a completely separate topic and it is long enough to deserve its own book.

We will also discuss optimization techniques and implement one method called spatial partitioning that splits the area into smaller ones based on object density.

As a side note, all the concepts in this chapter can be easily extrapolated to collisions in 3D.

### Detecting collisions

There are two main approaches to checking collisions:

- Discrete, or a posteriori
- Continuous, or a priori

For the discrete approach, we advance the state of the game items and then check if any of them are intersecting. It is a discrete simulation because we only do the evaluation at the end of each step. It is called a posteriori because it is done after the objects have moved. It is reactive. Most of the time we lack the exact point of contact; we only know that at the end of the simulation step the objects are colliding.

On the other hand, a continuous approach predicts the collision before applying the movement, based on the parameters of each object. It is calculated before the movement is performed. That's why it is called a priori. This method provides the exact point of contact and it is extremely useful when we require precision— such as for physics simulation.

The discrete method is generally one dimension simpler than the continuous one. This makes it much easier to understand and implement. It trades speed for precision.

### Note

Discrete collision detection is faster but less precise.

Since we are not going to use physics and we don't really care about the exact point of contact, we will use the discrete approach.

### Who can collide?

To be able to calculate GameObject collisions, we need to associate a shape (or body) to a GameObject. This association requires some information about the object on the screen, such as position on the x and y axes and also the width and height.

We are going to create a class named screenGameObject that extends from GameObject and will contain that information. In our game, Sprite is the only class that will extend from screenGameObject, but it may come in handy if you want to place non-visual items on the screen that trigger something when the player crosses them, which is a common technique in games.

![](img/35d01831c19688a44e50bce7c489007a_1362_0.png)

*GameObject classes hierarchy*

To collide with other objects you need a position on the screen and a size. ScreenGameObject will provide that.

All the logic for calculation collisions will be placed inside the ScreenGameObject class. The stub of the class is like this:

```
public abstract class ScreenGameObject extends GameObject {

    protected double mX;
    protected double mY;

    protected int mHeight;
    protected int mWidth;

    public Rect mBoundingRect = new Rect(-1, -1, -1, -1);

    public boolean checkCollision(ScreenGameObject otherObject) {
        return false;
    }

    public void onCollision(GameEngine gameEngine, ScreenGameObject sgo) {
    }
}
```

We take this opportunity to refactor the variables width, height, x, and y to have shorter, more generic names. We will continue working with this class throughout the chapter.

The method checkCollision is where the action happens. That method will have a different implementation depending on the shapes we use to calculate the collisions.

Finally, onCollision is used to trigger an action when a collision occurs.

### Updating GameEngine

To run discrete collision detection, we will use a method named checkCollisions during the execution of onUpdate on the GameEngine, so it will run every time the game objects are updated. We will place this method call after the update of the game objects and before the code that removes objects. Collisions are a typical reason why objects get removed.

Since only ScreenGameObjects can have collisions, we will create a special list containing them.

```
private List<ScreenGameObject> mCollisionableObjects;
```

And we will keep it updated whenever an object is added to or removed from the GameEngine, as we did for the separate layers in the previous chapter.

The code to check for collisions and to notify the game objects involved is as follows.

```
private void checkCollisions() {
    int numObjects = mCollisionableObjects.size();
    for (int i = 0; i < numObjects; i++) {
        ScreenGameObject objectA = mCollisionableObjects.get(i);
        for (int j = i + 1; j < numObjects; j++) {
            ScreenGameObject objectB = mCollisionableObjects.get(j);
            if (objectA.checkCollision(objectB)) {
                objectA.onCollision(gameEngine, objectB);
                objectB.onCollision(gameEngine, objectA);
            }
        }
    }
}
```

The code is as simple as a nested for loop that checks every ScreenGameObject against all the others. If a collision is detected, we will execute the method onCollision on both screen game objects involved in it, passing the object they have collided with as a parameter.

Keep in mind that this method has quadratic complexity, while all the other methods involved in onUpdate have linear complexity instead. Running collision detection is expensive.

### Note

Checking for collisions has quadratic complexity O(n²).

### Handling collisions

Regardless of the method we use to calculate the collisions, the actions we have to take for the player and the bullets are the same. Let's override the method onCollision for them.

In the case of the bullets, we have to check if the object it collides with is an asteroid and, if that's the case, we remove both objects from the GameEngine.

```
public void onCollision(GameEngine gameEngine, ScreenGameObject otherObject) {
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
