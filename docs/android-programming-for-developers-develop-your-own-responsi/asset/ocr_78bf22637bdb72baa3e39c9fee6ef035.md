To add layer support, we need to modify the GameEngine, StandardGameView, and SurfaceGameView classes. For convenience, we will also update GameObject to know its layer.

On the top level, we will handle the layers as a list of lists of game objects. This is the data structure that we will store in the GameEngine and pass to the implementations of GameView. We will keep the old mGameObject list as a simple way to iterate over all the GameObjects.

We initialize the layers inside the constructor of GameEngine. We will add a parameter to the constructor showing the number of layers we expect to have, so they can be precreated:

```
private List<List<GameObject>> mLayers = new
ArrayList<List<GameObject>>();

public GameEngine (Activity a, GameView gameView, int numLayers) {
[...]
for (int i=0; i<numLayers; i++) {
mLayers.add(new ArrayList<GameObject>());
}
}
```

The major changes are inside onUpdate:

```
public void onUpdate(long elapsedMillis) {
mInputController.onPreUpdate();
int numObjects = mGameObjects.size();
for (int i=0; i<numObjects; i++) {
mGameObjects.get(i).onUpdate(elapsedMillis, this);
}
synchronized (mLayers) {
while (!mObjectsToRemove.isEmpty()) {
GameObject objectToRemove = mObjectsToRemove.remove(0);
mGameObjects.remove(objectToRemove);
mLayers.get(objectToRemove.mLayer).remove(objectToRemove);
}
while (!mObjectsToAdd.isEmpty()) {
GameObject gameObject = mObjectsToAdd.remove(0);
addToLayerNow(gameObject);
}
., }
```