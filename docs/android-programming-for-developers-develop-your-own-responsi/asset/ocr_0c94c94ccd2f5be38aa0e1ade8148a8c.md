interface, so nothing else needs to be changed. We are going to use the SurfaceGameView from now on, but feel free to experiment with StandardGameView as well.

# Note

The layout is where we set which variant of GameView we are going to use.

Finally, inside GameFragment, we update the creation of the GameEngine by adding the GameView parameter:

```
GameView gameView = (GameView)
getView().findViewById(R.id.gameView);
mGameEngine = new GameEngine(getActivity(), gameView);
```

Now we have a GameEngine that relies on a GameView to do the rendering. We still need to update the GameObject class to make use of it.

Before we get to the GameObject class, let's take a moment to improve DrawThread as well.