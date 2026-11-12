
```
@override
void render(Canvas canvas) {
  canvas.drawRect(squarePos, squarePaint);
}
```

- 14. Next, we update the square position every frame based on its speed and direction, plus the time that has elapsed since the previous frame.

Then, if the position of the square has reached the edge of the screen, we can flip the direction of the square:

```
@override
void update(double deltaTime) {
  squarePos = squarePos.translate(squareSpeed * squareDirection * deltaTime, 0);
  if (squareDirection == 1 && squarePos.right > screenWidth) {
    squareDirection = -1;
  } else if (squareDirection == -1 && squarePos.left < 0) {
    squareDirection = 1;
  }
}
```

- 15. Now, we can run the example and see our simple green square animating from left to right, reversing its direction when it hits the side of the screen.

Now, we have gone through a simple animation example to show how easy it is to get started and to give you a feel for game programming with Flutter.

Feel free to play with the code, maybe changing the color of the square or adding more squares at a different position. In the next chapter, we will dig deeper into this code.

## Summary

In this chapter, we explained why Flutter and Dart are well suited to multiplatform game development. We explained the building blocks of games that we will focus on in each section of the book. Finally, we showed you a simple code example to play with.

In the next chapter, we will start using Flame, the game engine library that works with Flutter to add features related to game programming.

## Questions

- 1. What is the minimum constant frame rate that Flutter draws at?
- 2. What is the name of the graphics engine used by Flutter?
- 3. Which platforms can we support with Flutter?
- 4. What is Skia and what is it used for?
- 5. What types of compilation does Dart support and why are they beneficial?
- 6. Why is stateful hot reload beneficial for rapid game development?
- 7. Why is Dart's garbage collection beneficial for the smooth animation used in games?

## Chapter 2: Working with the Flame Engine

**Flame** is a game engine that is added, as a library, to your Flutter project. It provides us with modules that allow us to build our game. These include support for images and sprites, animations, audio, collision detection, and more advanced modules for 2D physics and tile maps.

In this chapter, we will focus on how to get started with Flame and gain an understanding of the basics of the game engine, including its assets, game loops, and components. It's important to know all of this so that you have a good understanding of the library and how everything fits together to make games with Flame.

Once you are familiar with the basics of Flame, you will be able to progress to the more advanced topics later in the book.

In this chapter, we will cover the following topics:

- Organizing the assets in your game
- Adding the game loop
- Working with components

## Technical requirements

To examine the sources mentioned in this chapter, you can download them at https://github.com/PacktPublishing/Building-Games-with-Flutter/tree/main/chapter02.

The Flame project is evolving very quickly, so please refer to the very latest documentation for any changes or new features at https://flame-engine.org/docs/#/. At the time of writing, the latest version is 1.0.0, which has been integrated with Flutter v3.0.0 onward.

To install the latest version to your project, please add the following to your `pubspec.yaml` file:

```
dependencies:
flame: 1.0.0
```

After saving the file, the dependency will be downloaded, and the Flame modules will become available. If you followed the example at the end of the previous chapter, then you will already have this set up, so please ensure you have already done this.

The newest versions of Flame can always be found in the `pub` repository at https://pub.dev/packages/flame/install.

### Organizing the assets in your game

Your game will include many assets, such as images, audio, fonts, maps, and game data.

As with Flutter, Flame supports the asynchronous loading of assets and caching. However, it also builds on top of that functionality to add features that are useful for images and audio, to help use and manage those assets effectively. For example, if you have a sprite sheet containing many frames of animation, you can load them and split them into their individual sprites very easily. This is covered, in more depth, in Chapter 4, Drawing and Animating Graphics.

All of your assets go under the `assets` folder in Flutter. I recommend the following folder structure to keep everything well organized:

```
assets
--- audio
-------- music
----------- music_menu.mp3
-------- sounds
------------- sound_shoot.mp3
-- images
-------- sprites
----------- sprite_player.png
----------- sprite_enemy.png
-- fonts
-------- font_highscore.ttf
-- maps
-------- map_level.tmx
```

If they are stored within the `assets` folder, Flutter and Flame allow you to organize your assets however you want.

In your `pubspec.yaml` file, the previous assets should then look like this:

```
flutter:
- assets/audio/music/music_menu.mp3
- assets/audio/sound/sound_shoot.mp3
- assets/images/sprites/player.png
- assets/images/sprites/enemy.png
- assets/fonts/font_highscore.ttf
- assets/maps/map_level.tmx
```

I recommend prefixing the asset type to make it very clear what the asset is. For instance, you might have a player sound and a player sprite, so having them named `sound_player.mp3` and `sprite_player.png` makes what they represent clearer.

Additionally, you should load any game assets for your current screen or level in advance at the start of the game or level. Flame has an `onLoad()` function that you can use to override this, which we will discuss further in the next section.

Flame has some helper functions to load the different asset types:

```
await Flame.images.load('player.png');
await FlameAudio.audioCache.load('explosion.mp3');
```

Here, the `load` function loads the asset into Flutter's internal memory cache for faster access. Also, it's important to use async/await as the assets are loading asynchronously, so we need to wait until the assets have been loaded before continuing.

Now that we understand how to add assets to our game, let's talk about how Flame constantly redraws the screen based on the current game state.

### Adding the game loop

The **game loop** controls any updates to the game state and then draws any graphics on the screen to reflect the current game state.

For instance, the player might move the character they are controlling to the right, which will then increase the _x_ position of the player's sprite during the game state update. Now that the game state has changed, the player will be drawn at the new position.

In a more complex game, hundreds of enemy sprites could also be moving around. Therefore, the state of these sprites also needs to be calculated.

This continues in a loop, where anything that is currently on the screen is updated and then redrawn. Each redraw is known as a **game frame**.

The number of frames drawn per second reflects how smooth the game is. In Flutter, apps and games redraw at 60 **frames per second (FPS)** to allow for very smooth redrawing.

In Flame, there are two functions that we can override to control the updating of the game state and drawing:

```
void update (double deltaTime)
void render (Canvas canvas)
```

Let's look at these functions in more detail.

### Update

In the `update` function, a parameter is passed called `deltaTime`, which tells us the time that has elapsed since the previous frame was drawn. We need this value to ensure our sprites run at consistent speeds across different devices. Devices run at different speeds depending on the processing power, so if we ignore the delta value and just run everything at the maximum speed
