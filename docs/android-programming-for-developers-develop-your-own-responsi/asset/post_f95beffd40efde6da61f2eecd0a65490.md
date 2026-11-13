
## Coding SurfaceView

Create a new class called GameView, extend SurfaceView, and implement Runnable.

Now, we will add a whole bunch of members:

- A thread called mGameThread that we will start and stop from the pause and resume methods that we will implement soon. These methods are of course called by the onResume and onPause methods of the MainActivity class.
- SurfaceHolder is what we need to allow us to do to our drawing.
- Volatile boolean mPlaying will be true when the thread is running and false otherwise. It will be used to determine whether we enter a while loop that will control the whole game loop. We have not seen the volatile keyword before, and we use it because mPlaying can be accessed from outside and inside the thread.
- We have a boolean mPaused variable, which will determine whether the game is currently paused.
- We have a Paint and Canvas object, which we will use in the same way we did in the Canvas Demo mini app.
- Next, we have a long variable, mFPS, which will hold the current number of frames per second that our game loop is achieving, and of course this is the value we will be passing in to the update methods of Paddle and Ball to allow them to move by the correct amount.
- Next, we declare mScreenX and mScreenY to hold the screen resolution, which as we saw is passed into the constructor from MainActivity when we instantiate a GameView object. We will code that constructor very soon.
- Now, we get to the neat stuff: a Ball object called mBall and a Paddle object called mPaddle.
- Next up, we have all the members that will take care of sound effects, including a SoundPool and four int IDs for sound effects.
- Finally, mLives and mScore will keep track of the player's score and how many lives he or she has left.

Code the GameView class and its members as we have just discussed:

```java
// Notice we implement runnable so we have
// A thread and can override the run method.
class GameView extends SurfaceView implements Runnable {
    // This is our thread
    Thread mGameThread = null;

    // This is new. We need a SurfaceHolder
    // When we use Paint and Canvas in a thread
    // We will see it in action in the draw method soon.
    SurfaceHolder mOurHolder;

    // A boolean which we will set and unset
    // when the game is running-or not
    volatile boolean mPlaying;

    // Game is mPaused at the start
    boolean mPaused = true;

    // A Canvas and a Paint object
    Canvas mCanvas;
    Paint mPaint;

    // This variable tracks the game frame rate
    long mFPS;

    // The size of the screen in pixels
    int mScreenX;
    int mScreenY;

    // The players mPaddle
    Paddle mPaddle;

    // A mBall
    Ball mBall;

    // For sound FX
    SoundPool sp;
    int beep1ID = -1;
    int beep2ID = -1;
    int beep3ID = -1;
    int loseLifeID = -1;

    // The mScore
    int mScore = 0;

    // Lives
    int mLives = 3;
}
```

The constructor has quite a lot of code, but we have seen much of it before, and
what is left is fairly straightforward.

We initialize mScreenX and mScreenY from the passed in screen resolution.

We initialize mOurHolder by calling getHolder, and we initialize mPaint by
calling the default Paint constructor.

Next, we instantiate our paddle and ball by calling their constructors and passing
in the screen resolution, as is required.

Almost all the rest of the code sets up the sound in exactly the same manner we
saw in Chapter 17, Sound FX and Supporting Different Versions of Android.

The final line of code calls the setupAndRestart method to start a new game,
and we will code that method shortly.

Add the GameView constructor we have just discussed:

```java
/*
When we call new() on gameView
This custom constructor runs
*/

public GameView(Context context, int x, int y) {
    // The next line of code asks the SurfaceView class to set up our object.
    super(context);

    // Set the screen width and height
    mScreenX = x;
    mScreenY = y;

    // Initialize mOurHolder and mPaint objects
    mOurHolder = getHolder();
    mPaint = new Paint();

    // A new mPaddle
    mPaddle = new Paddle(mScreenX, mScreenY);

    // Create a mBall
    mBall = new Ball(mScreenX, mScreenY);

    /*
    Instantiate our sound pool
    dependent upon which version
    of Android is present
    */

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        AudioAttributes audioAttributes = new AudioAttributes.Builder()
                .setUsage(AudioAttributes.USAGE_MEDIA)
                .setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
                .build();

        sp = new SoundPool.Builder()
                .setMaxStreams(5)
                .setAudioAttributes(audioAttributes)
                .build();
    } else {
        sp = new SoundPool(5, AudioManager.STREAM_MUSIC, 0);
    }

    try {
        // Create objects of the 2 required classes
        AssetManager assetManager = context.getAssets();
        AssetFileDescriptor descriptor;

        // Load our fx in memory ready for use
        descriptor = assetManager.openFd("beep1.ogg");
        beep1ID = sp.load(descriptor, 0);

        descriptor = assetManager.openFd("beep2.ogg");
        beep2ID = sp.load(descriptor, 0);

        descriptor = assetManager.openFd("beep3.ogg");
        beep3ID = sp.load(descriptor, 0);

        descriptor = assetManager.openFd("loseLife.ogg");
        loseLifeID = sp.load(descriptor, 0);

        descriptor = assetManager.openFd("explode.ogg");
        explodeID = sp.load(descriptor, 0);
    } catch (IOException e) {
        // Print an error message to the console
        Log.e("error", "failed to load sound files");
    }
    setupAndRestart();
}
```

Here is the setupAndRestart method that we first call from the constructor. We will also call this method at the end of every game to start a new one. The code calls the reset method on ball to position it for the start of a game and if necessary, resets the mScore and mLives variables to 0 and 3 respectively.

Add the setupAndRestart method to the GameView class:

```java
public void setupAndRestart() {

    // Put the mBall back to the start
    mBall.reset(mScreenX, mScreenY);

    // if game over reset scores and mLives
    if (mLives == 0) {
        mScore = 0;
        mLives = 3;
    }

}
```

Here, we have our run method, which is the code that is running in a thread. We have a while loop controlled by the value of our volatile boolean called mPlaying. This while loop wraps all the rest of the code inside the run method.

Inside the while loop, we get the system time in milliseconds (thousandths of a second) and initialize the startFrameTime variable with the result. Then, we check if the game is currently paused if(!mPaused), and if the game isn't paused, we call the update method. Note this is the update method of the GameView class, not the Ball or Paddle classes' update methods. We will code this method soon.

Next, we call the draw method, which will contain all our drawing code. Now, we calculate the time the frame took to execute by getting the current system time again and subtracting startFrameTime from the result. We then put the result into mFPS, which of course will be passed to the update methods of the Ball and Paddle classes when they are called.

The reason we wrap the last bit of code in if (timeThisFrame >= 1) is because
if timeThisFrame equals zero, trying to divide by zero will crash the app.

### Tip

Dividing by zero is a bad move in any language and on any platform. If you
want to know more, take a look at this article:
https://en.wikipedia.org/wiki/Division_by_zero.

Code the overridden run method we have just discussed:

```java
@Override
public void run() {
    while (mPlaying) {
        // Capture the current time in milliseconds in startFrameTime
        long startFrameTime = System.currentTimeMillis();

        // Update the frame
        // Update the frame
        if (!mPaused) {
            update();
        }

        // Draw the frame
        draw();

        /*
        Calculate the FPS this frame
        We can then use the result to
        time animations in the update methods.
        */
        long timeThisFrame = System.currentTimeMillis() - startFrameTime;
        if (timeThisFrame >= 1) {
            mFPS = 1000 / timeThisFrame;
        }
    }
}
```

Now, we can code the update method.
