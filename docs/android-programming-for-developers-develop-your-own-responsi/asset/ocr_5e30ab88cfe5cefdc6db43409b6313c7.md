# # UpdateThread

UpdateThread is a thread that continuously runs updates on the game engine. For each call to onUpdate, it provides the number of milliseconds since the previous execution.

The basic run method of the update thread is as follows:

```
@Override
public void run() {
    long previousTimeMillis;
    long currentTimeMillis;
    long elapsedMillis;
    previousTimeMillis = System.currentTimeMillis();

    while (mGameIsRunning) {
        currentTimeMillis = System.currentTimeMillis();
        elapsedMillis = currentTimeMillis - previousTimeMillis;
        mGameEngine.onUpdate(elapsedMillis);
        previousTimeMillis = currentTimeMillis;
    }
}
```

The thread stays in a loop for as long as the game is running. On each iteration, it will get the current time, calculate the elapsed milliseconds since the previous run, and call onUpdate on the GameEngine object.

While this first version works and is very simple to follow, it can only start and stop a game. We want to be able to pause and resume it as well.

To pause and resume the game, we need a variable that we read inside the loop to check when to pause the execution. We'll need to keep track of the elapsed milliseconds and discount the time spent paused. A simple way to do it is like this:

```
while (mGameIsRunning) {
    currentTimeMillis = System.currentTimeMillis();
    elapsedMillis = currentTimeMillis - previousTimeMillis;
    if (mPauseGame) {
        while (mPauseGame) {
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
```