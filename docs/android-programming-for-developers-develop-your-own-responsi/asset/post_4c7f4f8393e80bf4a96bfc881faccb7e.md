
once they are shown, while the immersive sticky mode will hide them after a couple of seconds have passed, returning to the real fullscreen. The recommended mode for games is to use sticky immersion.

The code to put the app in the fullscreen sticky immersion mode is as follows:

```java
@Override
public void onWindowFocusChanged(boolean hasFocus) {
    super.onWindowFocusChanged(hasFocus);
    if (hasFocus) {
        View decorView = getWindow().getDecorView();
        decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                | View.SYSTEM_UI_FLAG_FULLSCREEN
                | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
                | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);
    }
}
```

In this case, as in the previous one, we are requesting the use of a stable layout, and we are making it as if it is fullscreen. This time, we include a flag to make the stable layout the one with no Navigation bar (SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION).

We also add the flags to hide the Status bar (fullscreen) and the Navigation bar (hide navigation). Finally, we ask for the immersive sticky mode. The result is a real fullscreen game:

![](img/35d01831c19688a44e50bce7c489007a_1245_0.png)

Immersive mode gives us all the screen space on the device

With this configuration, even when the user does a gesture to show the Status and Navigation bars, they are shown in a semitransparent way overlaid on top of our UI:

![](img/35d01831c19688a44e50bce7c489007a_1246_0.png)

When the bars are shown while in sticky immersion mode, they are overlaid and semi transparent

Unfortunately, the sticky mode requires us to add the SYSTEM_UI_FLAG_HIDE_NAVIGATION flag to put the Navigation bar in the sticky mode. This has a very bad side-effect in the previous versions of Android, making the Navigation bar appear and disappear continuously as soon as you touch the screen, since this flag without the immersive mode means something different.

In addition to this, the SYSTEM_UI_FLAG_LOW_PROFILE flag does not have any effect on the versions in which the immersive mode is available. This makes sense, since it is considered a replacement and an improvement on it.

## Putting fullscreen together

Since we have two different modes for requesting fullscreen, one prior to KitKat (low profile) and one from KitKat (immersive mode), and the flags for hiding the Navigation bar do not play together nicely, we need to make a different configuration based on which version of Android the device is running on:

```java
@Override
public void onWindowFocusChanged(boolean hasFocus) {
    super.onWindowFocusChanged(hasFocus);
    if (hasFocus) {
        View decorView = getWindow().getDecorView();
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.KITKAT) {
            decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                    | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                    | View.SYSTEM_UI_FLAG_FULLSCREEN
                    | View.SYSTEM_UI_FLAG_LOW_PROFILE);
        } else {
            decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                    | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                    | View.SYSTEM_UI_FLAG_FULLSCREEN
                    | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
                    | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                    | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);
        }
    }
}
```

With this code, we give the expected game experience to each one of the Android versions; a low profile with a dimmed Navigation bar on the versions older than KitKat and the full-immersive mode on the newer devices.

## Good practices for game developers

In general, you should avoid premature optimization. This means, do not optimize your code unless you have a performance problem.

Nevertheless, in games, we have two methods (onUpdate and onDraw) for which the execution time is critical. So, we will be providing a few tips that should be enough to get performance under a reasonable threshold.

For the rest of the cases, your code will be probably good. If you find a performance problem, you should measure it carefully to find where the bottleneck is and only then optimize it. Most of the time, the problem is not where we think it is. Premature optimization can lead to a less readable code without significant improvement.

### Object pools

The creation and destruction of objects is an expensive operation that should be limited. This is one area where a real-time game is a lot more sensitive than an app.

Every time you create an object, the garbage collector has a chance to be run. In the old versions of Android, it meant that everything stopped for 200ms. While it is no longer this bad, it may still be noticeable.

### Note

We should avoid object creation as much as we can.

We want to avoid any expensive operation to be performed inside the onUpdate method—which must run as fast as it can—so we are going to take the creation and destruction of objects out of it.

The solution for this is a well-known software pattern called object pool.

Before we start the game, we will precreate the objects we are going to need and put them in a pool. The pool can be something as simple as a stack or list.

Instead of creating an object, we will pick one from the pool and initialize it. If the pool is empty, it means that we underestimated the number of objects. So as a lesser evil, a new instance of the object must be created.

Instead of destroying an object, we will put it back into the pool.

The fact that we have to return objects to the pool forces us to figure out when an object is no longer needed instead of just relying on the garbage collector to do that for us. While it requires a bit of effort, this mental exercise will improve the game performance and structure. If you have ever worked with C++, this should be easy-peasy for you.

We will use object pools for all the game objects in the code; this means enemies and bullets basically.

### Avoiding enhanced loop syntax in lists

Related to the object creation, we should avoid the use of an enhanced loop syntax in the lists. While the for-each syntax is easier to read, it creates an iterator on-the-fly, which makes the execution slower and gives the garbage collector a chance to be run.

In the case of the onUpdate method of GameEngine, we could have written it using the for-each syntax like this:

```java
public void onUpdate(long elapsedMillis) {
   for (GameObject gameObject : mGameObjects) {
      gameObject.onUpdate(elapsedMillis, this);
   }
}
```

But this is significantly slower than using the standard for loop syntax. This is why it looks like this instead:

```java
public void onUpdate(long elapsedMillis) {
   int numGameObjects = mGameObjects.size();
   for (int i=0; i<numGameObjects; i++) {
      mGameObjects.get(i).onUpdate(elapsedMillis, this);
   }
}
```

In the particular case of arrays, the enhanced syntax is as fast as the traditional one on the devices with the **JIT (just-in-time)** compiler—which should be the case for all devices nowadays—so there is no drawback in always using the default loop syntax instead of the enhanced one.

It is also important to use a variable for the size instead of requesting it for every iteration, which leads us to the next tip.

### Precreating objects

Related to the inefficiency of creating objects inside the onUpdate loop, we should always precreate the objects we are going to use.

A good example of this practice is the Runnable objects that are created inside the GameObject to run onRemovedFromGameUiThread and onAddedToGameUiThread.

We could create them on-demand inside the game engine as a part of addGameObject and removeGameObject, but it will be much less efficient.

### Accessing variables directly

As often as we can, we will use a direct variable access instead of using getters and setters. This is a good practice in general, since accessors are expensive and the compiler does not inline them.

In the case of games, it makes sense to extend this practice to variables of other classes. As we mentioned several times before, the execution time of onUpdate and onDraw is critical; a difference of just milliseconds counts. This is why, when variables from the game objects are accessed by other game objects, we make them public and work with them directly.

This is a bit counter-intuitive for Java developers, since we are used to encapsulating everything through getters and setters. In this case, efficiency is more important than encapsulation.
