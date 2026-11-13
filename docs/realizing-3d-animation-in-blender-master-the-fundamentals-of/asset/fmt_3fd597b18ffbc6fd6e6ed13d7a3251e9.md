
## Rendering a frame from the Video Sequencer

Your Video Sequencer should now have an Image Sequence strip that takes up the duration of the scene’s frame range:

![Figure 5.31: Image sequence strip in the Video Sequencer](image/B17149_05_31.jpg)

**Figure 5.31:** Image sequence strip in the Video Sequencer

That’s all the sequencing we need to do for this chapter!

So, how do we set Blender to use the image sequence in the Video Sequencer? Trick question – there’s nothing more we need to do! Under `Post Processing` in your scene’s `Output Properties`, use of the Video Sequencer is enabled by default:

![Figure 5.32: Post Processing settings](image/B17149_05_32.jpg)

**Figure 5.32:** Post Processing settings

`Sequencer` has no effect when there’s nothing in the Video Sequencer, but as soon as we add an image or video strip, Blender will bypass the 3D part of the active scene and instead render whatever is in the Video Sequencer.

You can try it yourself with `Render Image` (`F12`) now. These frames seem to render much faster than they did in the previous scene, don’t they? That’s because we’re not really rendering in the way we’ve come to understand the word. Although we’re technically using the same `Render Image` (`F12`) command, no 3D data is being evaluated this time. Because our current sequence is just one image sequence strip with no effects, Blender is pretty much just showing us the images we’ve already rendered.

> **Important note**
>
> Pay attention to which scene you are in, as it is easy to confuse a rendered frame in your initial scene with a “rendered” frame in your sequence scene.

Don’t use `Render Animation` (`Ctrl` + `F12`) yet! Because our sequence scene still has the same `Output Path` and `Format` as our initial scene, doing so will overwrite the images with themselves. Not helpful at all! We’ll need to establish a new `Output Path` and choose a proper video format.

## Output settings for video export

Now that our Video Sequencer is ready to output the frames we want, let’s return one more time to the `Output` panel in the `Output Properties` tab of the Properties Editor. This time, we’ll adjust the settings to output a video file.

### Setting the output path (again)

First, let’s change this scene’s `Output Path` so our video doesn’t get buried with all those image files:

1. Click once more on the folder icon next to the `Output Path` to invoke a file browser.
2. Select a convenient location in your filesystem for the video file we’re about to create.
3. Choose a file name for your video (anything will do) and click `Accept`.

I suggest using the parent directory (the folder that contains the folder that contains the image sequence). That way, the video file can live right next to the folder with the sequence that helped create it.

Now that our `Output Path` is out of the way, our final task is to choose the right video format.

### Using built-in FFmpeg

At first glance, the `File Format` menu seems to offer few options for video formats, but the available settings expand when we select `FFmpeg Video`.

It’s a bit of a misnomer to refer to FFmpeg as a format. FFmpeg is in fact a popular command-line utility for reading and writing a broad range of video formats. Don’t worry – you won’t have to learn any commands yourself; Blender’s built-in implementation of FFmpeg handles that for us.

The parameters for FFmpeg are in the `Encoding` sub-panel:

1. In `Output Properties`, select `FFmpeg Video` as the `File Format`.
2. Open the `Encoding` sub-panel and set `Container` to `MPEG-4`.

The default values for all the other properties may be left as they are. These settings will create an `.mp4` video, which will probably work on most websites and media players:

![Figure 5.33: Output settings for video](image/B17149_05_33.jpg)

**Figure 5.33:** Output settings for video

## Outputting a video

Press `Render` | `Render Animation` (`Ctrl` + `F12`).

That’s all there is to it! Because we’re “rendering” a pre-rendered image sequence this time, this job should not take more than a minute or two.

> **Tip**
>
> You can experiment with different containers, codecs, and quality settings to find the right combination of quality, file size, and support across different platforms and devices for your needs.

You’re done! What you choose to do with your final video file is up to you, but I recommend letting other people watch it. That’s usually the most satisfying way to view an animation. Otherwise, we wouldn’t have bothered to render anything at all!

# Summary

If you’ve rendered any still images in Blender before, much of what we covered in this chapter probably seemed redundant. It turns out that if you know how to render one frame in Blender and save it as an image, it’s not much harder to render a few hundred more and convert them to a video. That’s all a video is, really – just a bunch of frames displayed in rapid sequence to create the appearance of motion. Come to think of it, that’s what an animation is, too.

In closing part one of this book, you have completed the technical essentials of making an animation in Blender and rendering the final product. By realizing the core concept of animation as mere numbers and images that change one frame at a time, you can animate anything you like... in theory. In practice, you’ll probably want to read on to the next chapter, where we introduce a topic so dense it fills all of part two!

# Questions

1. What makes the camera important for rendering?
2. Why do render engines have names? What’s so special about them?
3. What are some ways of reducing the time it takes to render something?
4. Does `Render Image` (`F12`) produce different-looking images than `Render Animation` (`Ctrl` + `F12`)?
5. What does a lossy image format lose? Can we get it back?
6. What’s the difference between an image and an image file? Where in Blender might we encounter an image, but not an image file?
7. Why would we take the extra step of outputting an image sequence before converting it to a video?
8. Earlier, we defined the word “render” to mean generating 2D images from 3D data. In what cases does it mean something else in Blender?
9. Which was it that we rendered in this chapter: the animation, the scene, the frames, the images, or the image sequence?

# Part 2: Character Animation

Do you know how to walk? How to talk? Do you know how to stand, or sit, or pick something up with your hands? Do you know what face you’re making at this very moment? Are you sure it’s not weird?

If you answered “yes” to any of these questions, I don’t believe you. Of course, I believe you can do these things. I’m sure you’re very good at walking, talking, standing, sitting, picking things up, and making a not-weird face; you’re so good you do it all without thinking, and that’s the problem. You’re not thinking. We do an awful lot of things without knowing how. Nowhere is this more apparent than in the field of character animation.

A 3D character is a complex system of interdependently moving parts, none of which move by anything such as instinct, reflex, or muscle memory. Every part must be positioned and keyed deliberately by the animator, just as we did with the objects we animated in Part One, only this time by the hundreds. If we do a good job, the character will look lifelike, as if her movement is just as spontaneous and natural as our own, but this requires that we think, really think, about how a body moves frame by frame. It means trying to animate a character walking... and realizing we don’t know how to walk.

This part contains the following chapters:

- [Chapter 6](#B17149_06.xhtml#_idTextAnchor135), *Linking and Posing a Character*
- [Chapter 7](#B17149_07.xhtml#_idTextAnchor155), *Basic Character Animation*
- [Chapter 8](#B17149_08.xhtml#_idTextAnchor178), *The Walk Cycle*
- [Chapter 9](#B17149_09.xhtml#_idTextAnchor206), *Sound and Lip-Syncing*
