Thanks to the `Sprite` base class, this implementation is very easy. We just need to set the speed in the construction and then update the position on both axes using the speed and `elapsedMillis`.

Lastly, we check whether the object is out of bounds and, if so, we remove it from the engine and return it to the pool.

We can now compile and run and can see asteroids coming towards our spaceship.

![](bbox=[0.115, 0.286, 0.882, 0.613])

# More on the transformation matrix

You may have noticed that the asteroids looked quite static. This is because the image is always the same. We can improve this with a nice simple trick: by adding rotation to them.

The first thing we need to do is to update the `Sprite` class to make it handle rotation as a part of the configuration of the transformation matrix:

```
@Override
public void onDraw(Canvas canvas) {
    mMatrix.reset();
```