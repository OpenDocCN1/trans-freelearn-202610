+   5. The two sound effects we will play are when an enemy collides with George and when George walks or runs, controlled by the joystick or touch. If you have played the sounds that we downloaded, you will notice the enemy dying sound effect is short and the running sound effect is much longer. The challenge is that if George keeps moving longer than the sound effect lasts, we need to keep the sound playing by looping the sound.

Also, as mentioned earlier, if the game is put in the background, we need to pause the sound and then resume it from the same point if the user returns to the game. Because of this, we need to store a reference to AudioPlayer returned when playing the long-looped audio returned from the running sound effect, meaning that we have control over its playback.

Let’s create a couple of variables at the top of the George class that we have opened for tracking whether a user is moving and for accessing the audio player state:

```
bool isMoving = false;
late AudioPlayer audioPlayerRunning;
```

+   6. At the bottom of the onLoad function, let's load the sound effects into the audio cache:

```
await FlameAudio.audioCache.loadAll(
['sounds/enemy_dies.wav', 'sounds/running.wav']);
```

We use await on this line to wait until all the sound effect files are loaded into the cache.

+   7. At the bottom of the onCollision function after the code block that checks whether the other is a zombie or skeleton and sets the score, add the following code to play the enemy dies sound effect when we collide with an enemy:

```
FlameAudio.play('sounds/enemy_dies.wav');
```