
After changing the `File Format` to `TIFF`, you can leave the rest of the settings to their defaults:

![Figure 5.27: Example output settings](image/B17149_05_27.jpg)

Figure 5.27: Example output settings

**Tip**

When `Overwrite` is disabled, Blender will skip rendering a frame if the file for it already exists in the `Output Path`. This is useful for resuming a partially completed rendering job.

## Pre-render checklist

A common annoyance among 3D animators is to render something for hours, only to later discover that we made a mistake and have to render the whole thing over again – sort of like baking a cake with one missing ingredient. This happens to everyone eventually, but to avoid it as much as possible, here are some things we should verify before rendering an animation:

*   We’ve watched the animation several times in camera view in the 3D Viewport
*   At least one frame, when rendered with `Render Image` (`F12`), looks fine and did not take too long to render
*   In the `Output Properties`, the `Format` is correct and the `Output Path` is one that we recognize and can find later
*   The `.blend` file has been saved

If all of this is true, you’re ready to render your animation!

**Important note**

Like flooring the gas pedal in a car, rendering is a resource-intensive process that pushes your computer’s hardware to the limit of its capability. Make sure your computer is in a cool environment with good ventilation to avoid overheating. If you are rendering on a laptop, keep it plugged into a power source. Rendering will drain your laptop’s battery like nothing else!

## Rendering the animation

Ready? Press `Render Animation` (`F12`).

You are now rendering the animation. As mentioned before, this process takes some time. Since we’re using `Eevee`, it shouldn’t take forever, but it won’t be instantaneous either.

**Tip**

Blender’s interface does not “lock up” while rendering, so you can still click things and inspect settings in other windows. Always cancel or finish rendering before resuming normal work, though.

Using your operating system’s file browser, you can navigate to the location set by `Output Path` and watch it slowly fill up with image files:

![Figure 5.28: File Browser showing thumbnails of each image in sequence](image/B17149_05_28.jpg)

Figure 5.28: File Browser showing thumbnails of each image in sequence

Notice that each image file bears the frame number in its filename. This will be important later when we import the images back into Blender as a single sequence.

When the last frame in your animation has been saved, the render is finished.

## Viewing the rendered frames

Blender comes with its own internal media player that can play back the image sequence that was just rendered. We can invoke it with `Render` | `View Animation` (`Ctrl` + `F11`). When called, this operation searches the Output Path for what ought to have been rendered. Of course, this only works if you’ve rendered something. If it finds what it’s looking for, Blender’s internal media player will attempt to play back the recently outputted video or image sequence.

Viewing the image sequence this way helps us quickly preview the rendered image sequence, to determine if it is acceptable before converting it to a video file. Do not be alarmed if the resulting video appears to lag or jitter, though – Blender’s adorable little media player does not always playback everything at optimal performance. It doesn’t even have a time bar, but it’s trying its best!

**Tip**

After previewing the final product, it’s common to want to make changes and render your animation again. Rendering a second time is as easy as rendering the first time, but consider setting a new Output Path if you don’t want to overwrite the sequence in the old one.

Now that we’ve done the heavy lifting of rendering our scene to an image sequence, we can now convert those images to a single video.

# Converting an image sequence to a video

If we can play back our image sequence with `View Animation` (`Ctrl` + `F11`), don’t we already kind of have a video? Well, yes and no – a video may be little more than a sequence of images, but a sequence of image files alone will not be recognized by most media players, and is too large and unwieldy to be posted online or sent to anyone else. A proper video file contains all the frames in a single file and can apply both spatial and temporal compression methods to keep the video file down to a size much smaller than an image sequence.

Lots of professional video editing/encoding applications can convert an image sequence to a video file. For instance, I can use the video encoding utility `FFmpeg` to convert my sequence of `.tif` files to an `.mp4` file with a single command:

```
ffmpeg -r 24 -i render/%04d.tif my_video.mp4
```

You shouldn’t need to download a new program or learn a whole new command-line syntax just to make your first video, though. This is a Blender book, and Blender already has everything we need!

## The Video Sequence Editor

Blender comes with a `Video Sequencer`, which – compared to dedicated video editing applications – is both powerful and rudimentary (it can open multilayer EXR sequences, but it can’t always play things back at full speed).

![Figure 5.29: Example project in the Video Sequencer](image/B17149_05_29.jpg)

Figure 5.29: Example project in the Video Sequencer

You can access Blender’s Video Sequencer by switching the `Editor Type` of an area in your screen to `Video Sequencer`. If you like, you can also add a `Video Editing` workspace by clicking the `+` tab at the top of your screen for `Add Workspace` | `Video Editing` | `Video Editing`.

We’re going to dip our toe into this area so that we can import the image sequence we just rendered. After importing the image sequence as a strip in the Video Sequencer, we can then render to a video file, which will be our final product.

### Creating a scene for video sequencing

In order not to get our image sequence mixed up with the 3D scene that generated it, we should use a separate scene in our `.blend` file for video sequencing. To make things easier, we can add a new scene that starts out with the same settings as the one we already have:

1.  Find the `Scene` selector near the top-right corner of the screen.
2.  Click the `New Scene` button and choose `Copy Settings`.
3.  Instead of `Scene.001`, rename the new scene something that makes sense, like `Sequence`.
4.  Bring up a Video Sequencer in the UI, if you haven’t already.

![Figure 5.30: New scene with an empty Video Sequencer](image/B17149_05_30.jpg)

Figure 5.30: New scene with an empty Video Sequencer

Using `Copy Settings` gives our new scene identical `Output Properties` as the previous one – a quick way to ensure that both scenes have the same frame range and resolution. `Render Properties` will be the same as well, but as we’ll soon see, those properties no longer concern us.

### Importing the image sequence

Though we normally think of a scene in Blender as a 3D space with 3D objects viewable in the 3D Viewport, it can also merely serve as the container for a sequence in the Video Sequencer.

The elements in Blender’s Video Sequencer are called strips. Let’s add our image sequence `strip` now:

1.  Go to the `Start Frame` (this should be frame `1`). Doing this ensures that the strip, when added, will begin at frame `1`, just like the animation in our original scene.
2.  In the Video Sequencer, use `Add` (`Shift` + `A`) | `Image Sequence`.
3.  Using the invoked file browser, navigate to the directory that contains your image sequence.
4.  Select all the images in your sequence and click `Add Image Strip`.

**Tip**

Controls for navigating and moving strips in the Video Sequencer are largely identical to those of the `Timeline`.
