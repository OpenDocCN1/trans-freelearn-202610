
- `Output Format`: Choose your target speaker configuration here. Note that you'll also need to have a matching speaker configuration in your studio.
- `Sample Rate`: Select a sample rate between `44.1 kHz` and `192 kHz`.

Your new `Song` is now configured for spatial audio. If you need to change the configuration of your `Song` at a later time, go to `Song` on the top menu and click on `Spatial Audio…`. This will open a dialog box where you can modify the settings we discussed previously.

## Mapping speakers

Before we start working on your first `Song` with spatial audio, let's make sure that Studio One is configured to send each output channel to the correct speaker. This process is called `speaker mapping`, and you only need to do this once as long as you use the same speaker configuration. Let's do it:

1. On the top menu, click on `Song` and then `Song Setup`.
2. On the `Song Setup` window, click on the `Audio I/O Setup` tab.
3. Click on `Outputs`.
4. Now, click on the field to the right of `Main` or `Main Output` to reveal a list of available output formats, as shown in `Figure 14.2`:

![Figure 14.2: Mapping speakers for spatial audio](`image/B31512_14_2.jpg`)

Figure 14.2: Mapping speakers for spatial audio

1. On this list, select the option that matches your `Song`'s intended format. The window will now display the default speaker mapping for the format you selected:

![Figure 14.3: Default speaker mapping for a 7.1.4 configuration](`image/B31512_14_3.jpg`)

Figure 14.3: Default speaker mapping for a 7.1.4 configuration

1. Drag the speaker names (`L`, `R`, `C`, etc.) to the desired outputs on your audio interface. For example, if your subwoofer is connected to `Line Out 8` on your interface, click on `LFE` and drag it to `Line Out 8`.
2. Once you've assigned each output to the correct speaker, click on `Make Default` if you want Studio One to remember this configuration for your future `Songs`.
3. Click `OK`.

If you're working with Dolby Atmos and you'd like to use only headphones for monitoring, the `Headphones` section below the `Main` output (as seen in `Figure 14.3`) has got you covered. Just plug your headphones into the headphone jack on your audio interface, and you'll be good to go.

Awesome! You have just configured Studio One to work with spatial audio. Now, you're ready to create awesome three-dimensional experiences for the world to enjoy. Let's jump right in.

## Formatting tracks for spatial audio

Once you are in your new `Song`, the workflow for creating productions is no different from what we have already discussed in the preceding chapters. The only thing you need to watch out for is to select the correct configuration when creating new tracks. Let's take care of that right now:

1. Click the `Add Tracks` button or press `Ctrl` + `T`/`Cmd` + `T`. This will open the `Add Tracks` dialog box.
2. Click on the `Audio` tab.
3. Using the `Format` pull-down menu, select the format you'd like to use for this track. For most purposes, you'll want to choose `Mono` (dialogue, vocals, a single instrument, etc.) or `Stereo` (sound effects, ambient noises, background music, etc.):

![Figure 14.4: Creating a new track for spatial audio](`image/B31512_14_4.jpg`)

Figure 14.4: Creating a new track for spatial audio

1. Click `OK`.

From here on, continue to work with Studio One like you always have. The workflow will only change when you reach the mixing phase, which we will discuss in the next section.

# Mixing and exporting spatial audio material

Traditional stereo mixing involves creating a soundscape across two audio channels: left and right. Sounds are positioned between the left and right speakers using the pan control on each track or channel. This creates the illusion of depth but it's constrained to a single dimension. Spatial audio mixing, on the other hand, involves creating a three-dimensional sound field where sounds can move around the listener. Sounds are positioned in a 360-degree sphere, offering height, depth, and width. This calls for a more intricate approach to panning, and we'll kick off this section by exploring Studio One's spatial panning capabilities.

## Using the Surround Panner

Once you've created your `Song` and configured Studio One for spatial audio production as described earlier in this chapter, you'll notice that the pan controls for tracks and channels on the `Mix` window now look very different. This new type of pan control is called `Surround Panner`. Double-click on any channel's pan control (highlighted in `Figure 14.5`) to open the `Surround Panner` in its own window:

![Figure 14.5: The Surround Panner window](`image/B31512_14_5.jpg`)

Figure 14.5: The Surround Panner window

The top half of the `Surround Panner` window shows the target speaker configuration for your production. In `Figure 14.5`, my target speaker configuration is `7.1.4`, as indicated in the top-left corner of the `Surround Panner` window. So, there are seven speaker icons placed outside the circle, representing the speaker array that will be used in the listening environment. Right in the middle of the circle stands a human figure, representing the lucky soul who gets to enjoy our production. Studio One's instruction manual lovingly dubs it `The Listener`, so we'll keep the tradition alive and call it that, too.

In order to pan the audio on a channel to any point between the seven speakers we just mentioned, all you need to do is to grab the arrow (currently extending from `The Listener`'s nose toward the top of the circle) and point it to where you want the sound on that channel to come from. Moving the arrowhead closer to or farther away from `The Listener` will affect the signal's size. If you just want to change the position of the sound without changing its size, you can use the `Size` lock button at the top right (with a padlock icon) to lock the size at its current setting.

You can click on any of the bubbles on the circle (labeled `L`, `R`, `Ls`, `Rs`, etc.) and drag them to change the spread of audio on a channel. Bringing the bubbles closer together will make the sound tight and focused as if it's coming from a single spot. Spacing them farther apart will spread the sound over a wider spectrum, making the sound big and spacious.

The bottom half of the `Surround Panner` window will only be available if your output configuration has height speakers. This section of the window is used for controlling the `elevation` of the signal on a channel. You can place the sound at `The Listener`'s ear level, on the height speakers, or anywhere in between. Just click on the bubbles and drag them to control the elevation.

Everything on the `Surround Panner` window can be automated. Just select your favorite automation mode in the top-left corner of the window (currently labeled `Auto: Off`), hit play, and get creative. We talked about automation modes back in `Chapter 11`, so you might want to visit that chapter to brush up on your automation skills.

`Surround Panner` is the basic tool for positioning sounds when working with spatial audio, but there are further options available if you're working with Dolby Atmos. We'll explore those options next.

## Understanding beds and objects

Dolby Atmos expands the creative possibilities for sound design and mixing, enabling a more engaging and lifelike auditory experience. As we mentioned at the beginning of this chapter, part of this is achieved by adding height speakers to the equation, creating a fully immersive experience, including the vertical dimension.

Another key point in the Dolby Atmos paradigm is the introduction of two concepts: beds and objects. These are revolutionary concepts that will change the way you look at your mixes, so let's break them down:
