

```
// The new way
// Build an AudioAttributes object
AudioAttributes audioAttributes = new AudioAttributes.Builder()
.setUsage(AudioAttributes.USAGE_ASSISTANCE_SONIFICATION)
.setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
.build();

// Initialize the SoundPool
sp = new SoundPool.Builder()
.setMaxStreams(5)
.setAudioAttributes(audioAttributes)
.build();
}
```

The else block of code shown here will, of course, contain the code for the old way of doing things. No need for an AudioAttributes object; we simply need to initialize SoundPool (sp) by passing in the number of simultaneous sounds. The final parameter is for sound quality, and passing zero is all that we need to do. This is much simpler than the new way but also less flexible with regard to the choices we can make:

```
else {
  // The old way
  sp = new SoundPool(5, AudioManager.STREAM_MUSIC, 0);
}
```

Now that we have sound effects in the assets folder and a SoundPool object built in one way or another, we can go ahead and load up (decompress) the sound files in our SoundPool class.

## Loading sound files into memory

As we often do with methods that load or save files to disks, we are required to wrap our code in try-catch blocks. This makes sense because reading a file can fail for reasons beyond our control and as we are forced to do so, because the method that we use throws an exception and the code that we write will not be compiled otherwise.

Inside the try block, we declare and initialize an object of the AssetManager and AssetFileDescriptor types. AssetFileDescriptor is initialized here using the openFd method of the AssetManager object, which actually decompresses the sound file. We then initialize our ID (idFX1) at the same time as we load the contents of AssetFileDescriptor into SoundPool. The catch block simply outputs a message to the console to let us know if something has gone wrong. Note that this code is the same regardless of the Android version:

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

## Playing a sound

At this point, there is a sound effect in our SoundPool class and we have an ID by which we can refer to it.

This code is the same regardless of how we built the SoundPool object, and this is how we play the sound. Note that we initialize the nowPlaying variable with the return value from the same method that actually plays the sound. The following code, therefore, simultaneously plays a sound and loads the value of the ID that is being played into nowPlaying. We will see how this will be useful when we make a real mini app to play some sounds:

```
nowPlaying = sp.play(idFX1, volume, volume, 0, repeats, 1);
```

The parameters of the play method are: the ID of the sound effect, the left-hand side speaker volume, the right-hand side speaker volume, the priority over other sounds, the number of times to repeat the sound, and the rate/speed at which it is played (1 is the normal rate).

Just one more quick thing before we make the real app.

## Stopping a sound

It is also very simple to stop a sound when it is still playing with the stop method. Note that there might be more than one sound effect playing at any given time, so the stop method requires the ID of the sound effect to stop:

```
sp.stop(nowPlaying);
```

Now, we will go ahead and make some noise for real at the same time as we try out a UI widget that we haven't used yet.

## Introducing the Spinner widget

Of course, with all this talk of sound FX we need some actual sound files. You can download sound FX from various free websites, and you can also buy premium sound effects as well. The sound effects for this app are supplied in the download bundle and are in the assets folder of the Chapter 17/Sound Demo folder. But you might like to make your own.

## Making sound FX

There is an open source app called Bfxr that allows us to make our own sound FX. Here is a guide to making your own sound FX very fast using Bfxr. Grab a free copy from www.bfxr.net.

Follow the simple instructions on the website to set it up. Try out a few of these things to make cool sound FX.

## Tip

This is a seriously condensed tutorial. You can do so much with Bfxr. To learn more, read the tips on the website at the previous URL.

- 1. Run Bfxr, as shown:

![](img/35d01831c19688a44e50bce7c489007a_567_0.png)

- 2. Try out all the preset types, as shown in the next screenshot, which generate a random sound of that type. When you have a sound that is close to what you want, move to the next step:

![](img/35d01831c19688a44e50bce7c489007a_568_0.png)

- 3. Use the sliders to fine tune the pitch, duration, and other aspects of your new sound, as shown in the following figure:

![](img/35d01831c19688a44e50bce7c489007a_569_0.png)

- 4. Save your sound by clicking on the **Export Wav** button. Despite the text of this button, we can save the sound in formats other than .wav too:

![](img/35d01831c19688a44e50bce7c489007a_569_1.png)

- 5. Android works very well with sounds in the OGG format, so when you’re asked to name your file, use the .ogg extension at the end of the filename.

- 6. Repeat steps 2 to 5 to create three cool sound FX. Name them fx1.ogg, fx2.ogg, and fx3.ogg. We use the .ogg file format as it is more compressed than formats such as WAV. When you have your sound files ready, we can proceed with the app.

## Laying out Sound Demo UI

I will describe the parts of the project that we are getting used to a little more tersely than the previous projects. Every time there is a new concept, however, I will be sure to explain it in full. I guess you will be just fine dragging a few widgets onto RelativeLayout and changing their text properties.

With this in mind, complete the following steps. If you have any problems at all, you can copy or view the code in the chapter 17/Sound Demo folder of the download bundle:

- 1. Create a new project, call it Sound Demo, and choose Blank Activity. Leave all the other settings at their defaults and delete the Hello world! widget.
- 2. In this order, drag Spinner, SeekBar, and four Buttons from the palette and onto the layout while arranging them and setting their text properties, as shown in the next screenshot:

![](img/35d01831c19688a44e50bce7c489007a_572_0.png)

- 3. Use the following table to set the widget's properties that we will need in our Java code:

| Widget | Property to be changed | Value to set |
|---|---|---|
| Spinner | id | spinner |
| Spinner | spinnerMode | dropdown |
| Spinner | entries | @array/spinner_options |
| SeekBar | id | seekBar |
| SeekBar | max | 10 |
| Button (FX 1) | id | btnFX1 |
| Button (FX 2) | id | btnFX2 |
| Button (FX 3) | id | btnFX3 |
| Button (STOP) | id | btnStop |

Next, add the following highlighted code to the strings.xml file in the values folder. We used the array of string resources named spinner_options for the options property in the previous step. It will represent the options that can be chosen from our Spinner widget:

```
<resources>
<string name="app_name">Sound Demo</string>
<string name="hello_world">Hello world!</string>
<string name="action_settings">Settings</string>
<string-array name="spinner_options">
    <item>0</item>
    <item>1</item>
    <item>3</item>
    <item>5</item>
    <item>10</item>
</string-array>
</resources>
```

Run the app now, and you will not initially see anything new. If you click on the Spinner widget, however, then you will see the options from our string array called spinner_options. We will use the spinner to control the number of times a sound effect repeats itself when played, as shown in the following screenshot:

