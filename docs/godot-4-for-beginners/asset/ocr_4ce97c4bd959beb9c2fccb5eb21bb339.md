![](bbox=[0.32, 0.095, 0.68, 0.285])

Figure 10.25 - Setting up the stream for our audio player

Because **Autoplay** is set to true, the track will begin playing as soon as the level loads. If you want to stop the music, all these settings can be changed from the script attached to the level, and playing can be set to false.

Now that we’ve got background music up and running, let’s move on to adding sound effects to the game.

# Implementing a sound effect

Sound effects do not play automatically and continuously. Instead, they are triggered by or paired with an event. A good example of this is when the player jumps or collects something.

First, you will need to download a sound effect of your choice. You can visit **Open Game Art** (https://opengameart.org/) and search for a jump sound and a coin or collectible sound. This website is an excellent resource for free game assets, all made available under a Creative Commons CC0 license.

Once you have a suitable jump sound and a collection sound, add your files to the Audio folder in the Godot **FileSystem**. This is shown in Figure 10.26:

![](bbox=[0.3, 0.71, 0.7, 0.86])

Figure 10.26 - Adding sound effects to our Audio folder