```
}
@Override
public void setGameObjects(List<GameObject> gameObjects) {
    mGameObjects = gameObjects;
}

@Override
public void draw() {
    if (!mReady) {
        return;
    }
    Canvas canvas = getHolder().lockCanvas();
    if (canvas == null) {
        return;
    }
    canvas.drawRGB(0, 0, 0);
    synchronized (mGameObjects) {
        int numObjects = mGameObjects.size();
        for (int i = 0; i < numObjects; i++) {
            mGameObjects.get(i).onDraw(canvas);
        }
    }
    getHolder().unlockCanvasAndPost(canvas);
}
}
```

First, we have three constructors with different arguments that are intrinsic to SurfaceView. Note that they all include a call to set a Callback to the SurfaceHolder, which is also implemented by SurfaceGameView. This callback will inform us of when SurfaceView is ready or when things have changed.

The next methods are the implementation of the callback interface. Those are the methods that are called when the SurfaceView is created, modified, or destroyed. We store the status of the view to know whether it is ready or not, so that it can be used to draw. A View is ready any time after it is created until it is destroyed.

Then, we move into implementing the methods from GameView.

To set the GameObjects, we do exactly as we did for StandardGameView, also with the same implications when it comes to handling a reference.