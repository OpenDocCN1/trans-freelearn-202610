```
null);
}
}
```

When we start the game, we first have to stop the previous one. Even when the GameOverDialog is in front, the GameEngine is not paused or stopped. In fact we can see the parallax background moving behind the dialog. This is a nice effect and we want to keep it like that, but that means we need to stop the current game before starting a new one or we will have two engines running at the same time, which is terrible for performance and battery life.

Note that there is no creation of a Player in the initialization. This is something we removed when we made the GameController manage the lives, but it is worth highlighting this again.

On the other hand we need to be able to show a dialog from inside the GameController since the utility methods for showing dialogs are implemented on the base fragment. That is why we need to pass a parameter to the constructor and store it as a member variable.

```
public GameController(GameEngine gameEngine, GameFragment parent) {
 mParent = parent;
 [...]
}
```

```
@Override
public void onGameEvent(GameEvent gameEvent) {
 [...]
 else if (gameEvent == GameEvent.GameOver) {
 mState = GameControllerState.GameOver;
 showGameOverDialog();
 }
}
}
```

```
private void showGameOverDialog() {
 mParent.getActivity().runOnUiThread(new Runnable() {
 @Override
 public void run() {
 GameOverDialog quitDialog = new GameOverDialog(mParent);
 quitDialog.setListener(mParent);
 mParent.showDialog(quitDialog);
 }
 }));
}
```