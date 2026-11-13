AssetManager object, which actually decompresses the sound file. We then initialize our ID (idFX1) at the same time as we load the contents of AssetFileDescriptor into SoundPool. The catch block simply outputs a message to the console to let us know if something has gone wrong. Note that this code is the same regardless of the Android version:

```
try{
    // Create objects of the 2 required classes
    AssetManager assetManager = this.getAssets();
    AssetFileDescriptor descriptor;

    // Load our fx in memory ready for use
    descriptor = assetManager.openFd("fx1.ogg");
    idFX1 = sp.load(descriptor, 0);
}catch(IOException e){
    // Print an error message to the console
    Log.e("error", "failed to load sound files");
}
```

Now, we are ready to make some noise.

# # Playing a sound

At this point, there is a sound effect in our SoundPool class and we have an ID by which we can refer to it.

This code is the same regardless of how we built the SoundPool object, and this is how we play the sound. Note that we initialize the nowPlaying variable with the return value from the same method that actually plays the sound. The following code, therefore, simultaneously plays a sound and loads the value of the ID that is being played into nowPlaying. We will see how this will be useful when we make a real mini app to play some sounds:

```
nowPlaying = sp.play(idFX1, volume, volume, 0, repeats, 1);
```

The parameters of the play method are: the ID of the sound effect, the left-hand side speaker volume, the right-hand side speaker volume, the priority over other sounds, the number of times to repeat the sound, and the rate/speed at which it is played (1 is the normal rate).

Just one more quick thing before we make the real app