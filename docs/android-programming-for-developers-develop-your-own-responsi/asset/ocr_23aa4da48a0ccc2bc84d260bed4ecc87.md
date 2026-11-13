# # Composite GameObjects and GameEngine

Until now, the GameObjects that we have been using are a single entity. From now on, we will also have GameObjects that include other GameObjects. In particular, the ParticleSystems used by each object will be owned by them.

This implies that the addition and removal of GameObjects to the GameEngine must be updated. The object will have two methods to add and remove itself from the GameEngine and we can override this method when we use a composite object to take care of them.

Our change will be to stop using the addGameObject and removeGameObject methods from GameEngine and start using new equivalent methods such as addToGameEngine and removeFromGameEngine on the GameObject instead.

This implies that we should go over the code of the project and replace all their occurrences. This is especially important in the case of the GameController, where the asteroids are spawned, and the initialization of the GameEngine inside GameFragment, when we add the Player object:

```
```java
mGameEngine = new GameEngine(getActivity(), gameView, 4);
mGameEngine.setInputController(
    new CompositeInputController(getView(), getYassActivity()));
new ParallaxBackground(mGameEngine, 20,
    R.drawable.seamless_space_0)
    .addToGameEngine(mGameEngine, 0);
new GameController(mGameEngine).addToGameEngine(mGameEngine, 2);
new Player(mGameEngine).addToGameEngine(mGameEngine, 3);
new FPSCounter(mGameEngine).addToGameEngine(mGameEngine, 2);
```
```

Even though the use of the old methods would produce the same result on noncomposite objects, it is better to add all the GameObjects to the GameEngine in the same way to have consistent code.

The default implementations of addToGameEngine and removeFromGameEngine are actually extremely simple for a noncomposite GameObject:

```
```java
public void addToGameEngine (GameEngine gameEngine, int layer) {
    gameEngine.addGameObject(this, layer);
}
```
```

```
```java
public void removeFromGameEngine (GameEngine gameEngine) {
```
```