Note that we use the Logger class to print an error message, but we do not end the window initialization if the AudioManager initialization fails. Audio replay should be kept optional, and we continue without music and sound replay instead of failing the application startup process.

Loading the music tracks into the AudioManager can be done right after the initialization call:

```
if (mAudioManager.isInitialized()) {
    mAudioManager.loadMusicFromFolder("assets/music", "mp3");
}
```

Here, we are trying to load all MP3 files in the assets/music folder into the AudioManager if the initialization is successful. Again, we don’t fail if no music files are found in the asset folder.

Adding music tracks to the playlist

In the AudioManager class, support for loading MP3 and OGG files from the local folder assets/music into a playlist has been implemented. You can add your own music to the assets/music folder as the playlist will be populated when the application is started. In the Practical sessions section, a task is available to add a Refresh button to the UI, allowing you to add or remove music tracks at application runtime.

Finally, we need to clean up the AudioManager when shutting down the application. So, we add a call to the cleanup() method of AudioManager to the cleanup() method of the Window class:

```
void Window::cleanup() {
    mRenderer->cleanup();
    mAudioManager.cleanup();
    ...
}
```

Since we created the AudioManager in the Window class, we need to create callback functions to access the music and sound-effect functions from the renderer or the user interface.

# Using callbacks to make the AudioManager class available everywhere

By using these callback functions, changing the music track or playing footstep sounds can be done from anywhere in the code. As an example, we will use the functionality to play a specific music track here, given the track name as a parameter.