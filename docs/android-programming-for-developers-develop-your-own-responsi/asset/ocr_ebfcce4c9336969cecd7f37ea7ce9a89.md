The fact that GameObject knows the layer it has to be added to allows us to add it to the right layer in a single line of code. Note that we also add the GameObject to the list of all game objects.

Finally, the code that draws the game objects into the canvas has to be changed in both implementations of GameView. This is the place where the order of the layers is used to provide the drawing order:

```
int numLayers = mLayers.size();
for (int i = 0; i < numLayers; i++) {
    List<GameObject> currentLayer = mLayers.get(i);
    int numObjects = currentLayer.size();
    for (int j=0; j<numObjects; j++) {
        currentLayer.get(j).onDraw(canvas);
    }
}
```

Note that, inside each layer, the order of drawing is still the order of addition. Since we can now isolate the same type of items to the same layer, this should not be something to worry about any longer.

Also, note that the synchronization object is the same in the GameViews and in the GameEngine, now that synchronization is done using mLayers.