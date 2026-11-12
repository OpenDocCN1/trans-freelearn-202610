Next, we generate a number between `1` and `48`, because we don't want to draw coins on the water tiles. Later, we will add collision detection to the water tiles to prevent the player or enemies from walking on the water. By generating a number with these bounds, it will give us a location on the map related to the map array data.

We then take those numbers and multiply the number by `32`, which is the size of the map tile, and add `5` to get the `posCoinX` and `posCoinY` values, which is the location of the coin in pixels.

We then use these values to add a new coin component to the map at this position.

Note that we do all this in a loop that adds 50 coins to the map, some of which will be currently offscreen until we add the map navigation.

+   10. Now that we have our coins added to the map, let's continue and update the `George` class so that George can collect the coins.

+   11. Open the `george.dart` file and at the top of the file, add the following import:

```
import 'package:goldrush/components/coin.dart';
```

+   12. At the bottom of the `onLoad` function, update the line that loads the audio files into the audio cache to load the `coin.wav` audio file:

```
await FlameAudio.audioCache.loadAll(
['sounds/enemy_dies.wav', 'sounds/running.wav',
'sounds/coin.wav']);
```

+   13. Finally, at the bottom of the `onCollision` function, let's add a check for a collision with a coin – if we collide with a coin, it will remove the coin from the game, update our score by 20, and play the coin audio:

```
if (other is Coin) {
other.removeFromParent();
hud.scoreText.setScore(20);
}
```