# Ambient music

Similar to ambient sounds, ambient music helps to prevent dead silence in a game. Playing no sounds at all may be intended to build up tension, but running for hours and hours in silence through a game can become boring. Adding a music track with a style and tempo that fits the gameplay helps the player to enjoy even slow passages of the game.

# Adaptive music play

When the player moves into a different area in an open world or enters an environment with a different style, or another room, changing the music to match the new place is a good idea. The famous “hearing boss music” moment when entering a room, combined with the sound of doors locking, will create unforgettable moments for a player.

# Allowing custom music

An interesting option for music selection is to let the player add local music to the list of tracks played in the game or even replace the music entirely. Seeing the game’s character exploring a virtual world while listening to some favorite music tracks may be a plus for players.

To experience the difference audio replay can make, we will add a C++ class to manage sound effects and music replay.

# Hands-on: Implementing an audio manager

The audio manager class consists of two distinct parts. On one hand, we need a high-level interface between the audio manager and the renderer and other classes. On the other hand, the encapsulated low-level interface linking the audio manager functionality to the audio library of the operating system would allow us to replace the audio library with another variant.

We will start with a brief overview of the high-level interface and dive into the low-level implementation afterward.

# Defining the high-level interface

The new AudioManager class, consisting of the AudioManager.h header file and the AudioManager.cpp implementation file will be added to the tools folder. We will add the basic functions a usual music/sound player should have:

+   - Initializing and cleaning up the audio library
+   - Loading music tracks from files on the file system
+   - Loading sound effects from files on the file system