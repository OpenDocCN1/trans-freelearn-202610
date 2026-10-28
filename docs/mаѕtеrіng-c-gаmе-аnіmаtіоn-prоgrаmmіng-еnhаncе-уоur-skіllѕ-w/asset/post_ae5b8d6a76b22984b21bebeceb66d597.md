
At the termination time of the application, the cleanup() method of the AudioManager is called by the cleanup() method of the Window class. Inside the cleanup() method of the AudioManager class, we close the audio device and signal both SDL_mixer and SDL to run their respective cleanup code:

```
Mix_CloseAudio();
Mix_Quit();
SDL_Quit();
```

Note that we did not call Mix_Init() in the init() method, but we have to call Mix_Quit() here. SDL manages the mixer initialization for us without an explicit call to Mix_Init().

Now we are ready to play music and sound effects. Let’s start with a look at the music replay code.

## Controlling music replay

Loading a music track is done by calling Mix_LoadMUS() with the C-style character array as a parameter:

```
Mix_Music* music = Mix_LoadMUS(fileName.c_str());
```

SDL_mixer supports several music formats, such as WAV, MP3, and OGG. The return value of Mix_LoadMUS() is a pointer to the Mix_Music struct.

To clean up a loaded music track, Mix_FreeMusic() must be called with the pointer to the above Mix_Music struct:

```
Mix_FreeMusic(music);
```

A music track can be played by calling Mix_PlayMusic() with two parameters:

```
Mix_PlayMusic(music, 0);
```

The first parameter is a pointer to a valid Mix_Music struct, while the second parameter tells SDL_mixer the number of loops to play. A loop value of 0 disables looping and a value of -1 will loop the music infinitely.

Stopping the music is done by calling Mix_HaltMusic():

```
Mix_HaltMusic();
```

Since there is only one music track playing at a time, no parameters are needed, and in case the current track should be paused or resumed, the two Mix_PauseMusic() and Mix_ResumeMusic() calls are available:

```
Mix_PauseMusic();
Mix_ResumeMusic();
```

Finally, the music volume is controlled by Mix_VolumeMusic():

```
Mix_VolumeMusic(volume);
```

The parameter for Mix_VolumeMusic() is an int value between 0 and 128 to set the new volume, or a value of -1 to query the current volume of the music.

By using the default SDL_mixer calls, we can only play one music track once or with an infinite number of loops. If a dynamic music system is required, a manual implementation is needed, or even a different sound replay API should be considered. For the simple ability to advance to the next track in a playlist at the end of a track, we just need to implement a callback.

## Adding a callback for continuous music playback

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

We only want to add sound effects at the same time as the walking and running animations replay.

Then we check for the movement state of the instance and call the proper callback for the state:

```
switch (state) {
    case moveState::run:
        mModelInstCamData.
            micPlayRunFootstepCallbackFunction();
        break;
    case moveState::walk:
        mModelInstCamData.
            micPlayWalkFootstepCallbackFunction();
        break;
```

When the instance is in the running state, we play the sound effect for running footsteps. And if the instance is in the walking state, we play the walking footsteps.

If the instance is neither running nor walking, we stop the footstep sounds:

```
default:
    mModelInstCamData.
        micStopFootstepCallbackFunction();
    break;
}
```

And if the instance is not in the idle/walk/run cycle, we also stop any sounds. This way, we catch all the actions like jumping or punching where no sound effect is available yet:

```
} else {
    mModelInstCamData.
        micStopFootstepCallbackFunction();
}
```

To hear the footsteps sounds, the following steps are required:

- 1. Start the application and load a configuration with animation mapping.
- 2. Select (or create) a first-person or third-person camera.
- 3. Select an instance and set the instance by clicking on Use Selected Instance in the Camera section of the Control window of the UI.
- 4. Press F10 to switch to view mode.

If you move around the controlled instance now by using the mouse and the W/A/S/D keys, you should hear two different footstep sounds, depending on the instance walking or running around in the virtual world.

## Extending the audio manager class

To outline a possible expansion of the AudioManager class to support more channels to achieve a more game-like sound management, we would need to keep track of the channel playing the footstep sound returned by the Mix_PlayChannel() call. By reserving one channel exclusively for the local footstep sounds, we can achieve the same behavior, but we would be able to play more sound effects at the same time.

Handling multiple sound effects can be achieved by adding a creating a pool of sound channels and adding a callback to the Mix_ChannelFinished() SDL function, similar to Mix_HookMusicFinished(). SDL triggers the Mix_ChannelFinished() callback whenever a channel has finished the current sound clip, or when Mix_HaltChannel() is called and delivers the number of the finished sound channel in the callback.

The sound channel pool can be updated when a sound effect is played and when the effect replay has finished. By using the distance to the object creating the sound effect and scaling down the volume of the channel with Mix_Volume() or by using Mix_SetDistance(), the different distances of the source can be modeled. In addition, Mix_SetPanning() can be used to adjust the position of the sound source to the left and right.

Several tasks in the Practical sessions section are available to evolve the sound replay from the current state.

As an example of how to use the AudioManager callbacks from the UserInterface class, a simple music replay control has been added to the user interface. In the Music & Sound section of the Control window, you will find a combo box and some buttons to play the music from the playlist created from the assets/music folder.

## Using the music player in the UI

Adding music replay functionality to the UserInterface class is a quick and simple task. By using the callbacks to the AudioManager class, a basic music player is implemented with only a few code blocks to the UI.

In Figure 14.2, the ImGui section with a basic music player using the AudioManager class is shown:

![](img/3cfb9832c943f5f7366fd104b5255971_498_0.png)

Figure 14.2: A music player based on the AudioManager added to the user interface

The desired music track to play can be chosen by using the Tracks combo box, the current track can be played by pressing the Play button, or a random track from the playlist will be chosen from the shuffled playlist by pressing Play Random. The other four buttons, Prev, Pause, Stop, and Next, do exactly what the label states, and by using the sliders, the volume of music and sound effects can be set to a value between 0 (for silence) and 128.

## Alternative sound manager implementations

The AudioManager class used for the application uses a straightforward implementation with direct control through C++ callback functions. For an advanced sound system, an event- or message-based implementation could be used. Such an implementation can use event managing code already in the game, and events or messages also decouple the sound effects replay from the code that is requesting to replay a sound effect or a music change.

After having sound and music available in the virtual world, an update to the graphics of the application may appear also on the to-do list. Let’s explore some visual enhancements now.

## Enhancing visuals

The OpenGL and Vulkan renderers in the example code support only a minimal set of features to bring an image to the screen: Both renderers can only draw textures triangles and colored lines. Adding one or more of the following enhancements will be a fantastic extension to the quality of the images.

## Bringing colors to the world by using physically based rendering

Right now, we are using only the textures of the Assimp objects to render the model instances and level geometry to the screen. By using physically based rendering, short PBR, we could also model surface properties in the form of materials. With such PBR materials, it is easy to create
