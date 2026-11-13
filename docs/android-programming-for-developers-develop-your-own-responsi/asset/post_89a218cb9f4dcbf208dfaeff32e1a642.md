

## Updating GameEngine

The use of GameView has some implications from the GameEngine point of view.
It means that it has to initialize the GameView and then trigger draws using the
generic interface.

The GameView will be a parameter of the constructor of GameEngine. It will be
initialized, passing a reference to the list of game objects. The updated
constructor of GameEngine is like this:

```
public GameEngine (Activity a, GameView gameView) {
mActivity = a;
mGameView = gameView;
mGameView.setGameObjects(mGameObjects);

mWidth = gameView.getWidth()
- gameView.getPaddingRight() - gameView.getPaddingLeft();
mHeight = gameView.getHeight()
- gameView.getPaddingTop() - gameView.getPaddingBottom();

mPixelFactor = mHeight / 400d;
}
```

From now on, we will also calculate the pixelFactor inside the GameEngine.
We will store it in a public variable so it can be read by the game objects. This
has several advantages, such as:

- If we decide to change the number of units of the screen, this is done in a single place
- Removing code duplications is always good for maintenance

On the other hand, the onDraw method of the GameEngine becomes extremely
simple:

```
public void onDraw() {
mGameView.draw();
}
```

## Updating the game layout

Of course, we have to modify the fragment_game.xml layout to include the GameView. We will take this chance to do some other modifications to it, such as removing the TextView and changing the padding of the layout to be the margins on the pause button instead. This makes sure that the GameView is fullscreen while keeping the button margins as they were.

It is important to remember that in a FrameLayout, the order in the XML specifies the order in which the items are drawn (the z-index). We will put the GameView at the beginning of the layout to have the pause button drawn on top of it.

The new version of fragment_game.xml is as follows:

```xml
<FrameLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical"
tools:context="com.example.yass.counter.GameFragment">

<com.example.yass.engine.SurfaceGameView
android:id="@+id/gameView"
android:layout_width="match_parent"
android:layout_height="wrap_content" >

<Button
android:layout_gravity="top|right"
android:id="@+id/btn_play_pause"
android:layout_marginTop="@dimen/activity_vertical_margin"
android:layout_marginRight="@dimen/activity_vertical_margin"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/pause" >

<include layout="@layout/view_vjoystick" />

</FrameLayout>
```

Note that this is the place where we decide which variant of the GameView we are going to use. The rest of the code will access the methods via the GameView interface, so nothing else needs to be changed. We are going to use the SurfaceGameView from now on, but feel free to experiment with StandardGameView as well.

### Note

The layout is where we set which variant of GameView we are going to use.

Finally, inside GameFragment, we update the creation of the GameEngine by adding the GameView parameter:

```
GameView gameView = (GameView)
getView().findViewById(R.id.gameView);
mGameEngine = new GameEngine(getActivity(), gameView);
```

Now we have a GameEngine that relies on a GameView to do the rendering. We still need to update the GameObject class to make use of it.

Before we get to the GameObject class, let's take a moment to improve DrawThread as well.

## Improving DrawThread

To trigger the draw we have been using a `Timer` and `TimerTask` scheduled so we could get 30 frames per second. While this works, it gives better performance to do it like the `UpdateThread`: run as many calls to `onDraw` as we can.

This approach works great for `SurfaceView` since the drawing is done on the same thread. But it may give some message overflow problems while using `StandardGameView`, which just calls `postInvalidate`. To prevent the overflow, we will ensure that the time between the calls to `onDraw` is never shorter than 20 milliseconds, which is enough for 50 frames per second.

The code for the new `DrawThread` is exactly the same as for `UpdateThread`, except for the part in the run method that takes care of the overflow. It looks like this:

```
@Override
public void run() {
long elapsedMillis;
long currentTimeMillis;
long previousTimeMillis = System.currentTimeMillis();

while (mGameIsRunning) {
currentTimeMillis = System.currentTimeMillis();
elapsedMillis = currentTimeMillis - previousTimeMillis;
if (mPauseGame) {
while (mPauseGame) {
try {
synchronized (mLock) {
mLock.wait();
}
} catch (InterruptedException e) {
// We stay on the loop
}
}
}
currentTimeMillis = System.currentTimeMillis();
}
if (elapsedMillis < 20) { // This is 50 fps
try {
Thread.sleep(20-elapsedMillis);
} catch (InterruptedException e) {
// We just continue.
}
}
mGameEngine.onDraw();
previousTimeMillis = currentTimeMillis;
}
}
```

In the case of running two calls too close, we put the thread to sleep from the remaining time to a minimum of 20 milliseconds.

If an InterruptedException happens, we don't really have much to handle. So, we can just move on and call onDraw in the GameEngine.

## Sprites

We have already mentioned the concept of a sprite being an entity that is drawn and handled at a specific position on a screen. Essentially, everything we see in a game are sprites. There are exceptions, such as game controllers (which do not draw anything) and backgrounds (which are drawn in a different way), but we will talk about them later in the chapter.

So, this is the code for the Sprite class that extends GameObject:

```
public abstract class Sprite extends GameObject {
protected double mPositionX;
protected double mPositionY;
protected final double mPixelFactor;
private final Bitmap mBitmap;
protected final int mImageHeight;
protected final int mImageWidth;
private final Matrix mMatrix = new Matrix();
protected Sprite (GameEngine gameEngine, int drawableRes) {
Resources r = gameEngine.getContext().getResources();
Drawable spriteDrawable = r.getDrawable(drawableRes);
mPixelFactor = gameEngine.mPixelFactor;
mImageHeight = (int)
(spriteDrawable.getIntrinsicHeight()*mPixelFactor);
mImageWidth = (int)
(spriteDrawable.getIntrinsicWidth()*mPixelFactor);
mBitmap = ((BitmapDrawable) spriteDrawable).getBitmap();
}
@Override
public void onDraw(Canvas canvas) {
mMatrix.reset();
mMatrix.postScale((float) mPixelFactor, (float) mPixelFactor);
mMatrix.postTranslate((float) mPositionX, (float) mPositionY);
canvas.drawBitmap(mBitmap, mMatrix, null);
}
}
```

Sprite is an abstract class and there is no implementation of onUpdate at all.
Sprites care about displaying an item on the screen, not about how this item
moves.

The class has a series of member variables. Let's go through them:

- mPositionX, mPositionY: The position of the Sprite on the screen. The same concept we used for the Player and the Bullet objects. This position is in the top-left corner of the image.
- mPixelFactor: The same concept as before. The factor to convert screen units into pixels.
- mBitmap: The bitmap we are going to draw.
- mImageWidth, mImageHeight: The size of the bitmap as drawn on the screen. It is set here for convenience. We could always calculate it using the bitmap and the pixel factor, but it is faster to just store it for future use by the subclasses.
- mMatrix: This object is a transformation matrix. It is used for scaling, moving, and rotating the bitmap before rendering it on the canvas. For optimization purposes, it is reused among the onDraw executions instead of being created for each run.

The next piece of code is the constructor. We are passing a reference to the GameEngine and a drawable resource. In the previous chapter, we were passing the parent View, so we could add the newly created ImageView to it. This is no longer necessary. We just need the context and the pixel factor, which we can get from the GameEngine.

The code in the constructor is very similar to the ones we have seen before. It does several things:

- Loads the Drawable from resources via the context
- Gets the intrinsic size of the drawable and multiplies it by the pixel factor to store the width and height that the sprite will use in pixels for this particular device
- Gets the Bitmap from the Drawable and stores it in a class variable to use it during onDraw

Finally, we have the onDraw method that receives a Canvas. This method is now called from the GameView. The Canvas is obtained in a different way from a

