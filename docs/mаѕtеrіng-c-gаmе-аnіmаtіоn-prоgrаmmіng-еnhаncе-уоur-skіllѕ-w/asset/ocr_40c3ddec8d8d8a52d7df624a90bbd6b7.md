Finally, the music volume is controlled by Mix_VolumeMusic():

```
Mix_VolumeMusic(volume);
```

The parameter for Mix_VolumeMusic() is an int value between 0 and 128 to set the new volume, or a value of -1 to query the current volume of the music.

By using the default SDL_mixer calls, we can only play one music track once or with an infinite number of loops. If a dynamic music system is required, a manual implementation is needed, or even a different sound replay API should be considered. For the simple ability to advance to the next track in a playlist at the end of a track, we just need to implement a callback.

# Adding a callback for continuous music playback

SDL_mixer supports a callback to inform the application that the current music track has finished the number of loops. In this callback, we can simply forward to the next track in the playlist.

Sadly, SDL_mixer is a C audio library, and the callback must fulfill the C-style calling convention. The C++ calling convention is not compatible with the C calling convention and allows only calling static member functions of the AudioManager class. We need to add a small hack to be able to call a non-static method of the AudioManager class that has access to the playlist and the current position in the playlist.

First, we declare a raw pointer named mCurrentManager as private member variable, plus the private static staticMusicFinishedCallback() member method for the C-style callback and a private non-static member musicFinishedCallback() for the translated callback:

```
static AudioManager* mCurrentManager;
static void staticMusicFinishedCallback();
void musicFinishedCallback();
```

In the init() method of the AudioManager class, we set the mCurrentManager pointer to the current instance and call the callback hook setup method Mix_HookMusicFinished() with the static callback method:

```
mCurrentManager = this;
Mix_HookMusicFinished(staticMusicFinishedCallback);
```

Whenever a music track ends now, staticMusicFinishedCallback() is called by SDL_mixer. To translate the callback to C++, we use the pointer to the current AudioManager instance stored in mCurrentManager to call the non-static callback musicFinishedCallback() method:

```
if (mCurrentManager) {
```