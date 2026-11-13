
The stereo width control uses a sophisticated technique referred to as mid-side processing to adjust the perceived width of a stereo signal. **Mid-side processing** manipulates the stereo image of an audio signal by splitting it into two components: the mid component, which represents the audio signal that is identical in both the left and right channels, and the side component, which contains the audio signal that is different between the left and right channels. By processing these components separately, it is possible to adjust the stereo image of the audio signal in a number of ways.

In Studio One’s **Binaural** panner, this can be achieved by using the stereo width control. When this is set to **100%**, its default value, the stereo width of the signal on that track is not affected. Decreasing the value emphasizes the mid component of the signal, effectively turning it to mono at the lowest setting of **1%**. Increasing the value above **100%** emphasizes the side component of the signal, progressively widening the stereo image of the sound on that track. While this may instantly make your mix sound bigger, resist the temptation to crank the width up to its maximum level of **200%** since this may cause phase issues and make a mix unusable in certain contexts. We’ll cover phase issues and how to spot them in [*Chapter 15*](#B31512_15.xhtml#_idTextAnchor253), when we talk about mastering.

So far, we have discussed volume and panning, the two most important tools used for placing sounds on a virtual, three-dimensional sound stage in a mix. Setting these correctly is essential for crafting a coherent mix, but for most genres of music today, you will want these settings to change from one section of the song to the other, to create dynamism and contrast. That’s achieved by using automation, and that will be our next topic.

## Adding automation to create a dynamic mix

**Automation** is a powerful tool in music production that allows you to control various parameters of a mix, such as volume, panning, and effects, over time. Automation can be used in a variety of ways to create contrast between sections of a song and create interesting and dynamic effects.

Here are a few typical examples of where you could use automation:

*   Bring background vocals up during the chorus of a song.
*   Reduce the volume of a lead guitar during a quiet section.
*   Increase the amount of reverb on the lead singer’s voice in the bridge section.
*   Emphasize a word sung by the lead singer that is masked by a cymbal hit on the drums.

Each and every parameter on the mix console and the effects plug-ins can be automated in Studio One, so the only limit is your imagination. Now, let’s take a look at how you can work with automation.

### Automation modes

Every track and channel on the `Mix` window has an `Automation Mode` selector at the bottom, and every plug-in window has the same selector in its upper-left corner:

![Figure 11.15: Automation Mode selector on a track (left) and on a plug-in window (right)](image/B31512_11_15.jpg)

Figure 11.15: Automation Mode selector on a track (left) and on a plug-in window (right)

Clicking on this selector will reveal five options. Let’s take a look at these options one by one:

*   `Auto: Off`: This option disables automation for that track or plug-in. All parameters will remain locked in their current positions.
*   `Read`: In this mode, Studio One will read and execute all existing automation data on that track.
*   `Touch`: When you select this mode and press `Play`, new automation data will be created as soon as you touch a parameter on that track with your mouse or an external hardware controller. When you let go of your mouse or controller, Studio One will go back to reading and executing the existing data. This is an excellent method to make finer adjustments to an already existing automation.
*   `Latch`: This mode is similar to the `Touch` mode except that when you touch a parameter in this mode, Studio One will continue to add automation data until you stop playback.
*   `Write`: When you select this option and press `Play`, automation will be continuously written based on the current values of all parameters. If you already have automation data on that track, it will be overwritten. Use this method with caution to avoid accidentally overwriting your previous adjustments.

Now that you know about the automation modes, we can go ahead and start adding automation data.

### Adding automation

Adding automation is fairly straightforward. To illustrate with an example, let’s assume that you are mixing the song in Figure 11.16 and you want to increase the volume of the first track (labeled `Synth`) gradually between bars **8** and **9**, in order to build some excitement in anticipation of the song’s verse:

![Figure 11.16: A track before adding automation](image/B31512_11_16.jpg)

Figure 11.16: A track before adding automation

Here’s how you can do it:

1.  Open the `Mix` window and locate the fader for this track.
2.  Click on the `Automation Mode` selector below the fader and select `Touch`.
3.  Place the timeline to a point before bar **8** and press the `spacebar` to play back the project.
4.  As the timeline reaches bar **8**, click on the fader for that track and drag it up. Don’t worry if you don’t get it perfect – we’ll edit it in the next section, titled Editing automation.
5.  Press the `spacebar` again to stop playback.
6.  Click on the `Automation Mode` selector again and, this time, set it to `Read`. This will prevent you from accidentally adding or overwriting any automation data.

The changes you made to the fader are now written as automation data on that track. You can do another pass and, this time, add automation for another parameter. Automation data for each parameter is stored on separate lanes, which can be edited independently, as we are about to see.

### Editing automation

All the automation data you add to a track is stored on individual automation lanes. To reveal these lanes, right-click on a track’s header, and on the pop-up menu, select **Show/Hide Automation**. This will overlay a visual representation of all the automation data for a given parameter, referred to as an **automation envelope**. Here’s the automation envelope for my attempt to raise the volume of the track in Figure 11.16:

![Figure 11.17: Automation envelope displayed on an automation lane](image/B31512_11_17.jpg)

Figure 11.17: Automation envelope displayed on an automation lane

It doesn’t look very smooth, but it’s very easy to edit. Here’s how:

1.  Click on the bottom of that track’s header and drag down to enlarge the track. This will give you more room to work with.
2.  Click and hold the `Paint` tool to see its list of options. Select an option that serves your purpose (`Line` or `Parabola` would work perfectly for this example).
3.  Click and drag at the beginning of bar **8** and let go at the beginning of bar **9** to create a smooth line or parabola:

![Figure 11.18: Automation envelope after editing](image/B31512_11_18.jpg)

Figure 11.18: Automation envelope after editing

Other options for the `Paint` tool, such as `Sine` or `Square`, can be used to achieve more creative or experimental results with automation, such as continuously panning a sound between two speakers – definitely worth a try if your musical style has room for such experimentation.

This concludes our coverage of the two most basic and essential tools used in mixing: volume and panning. Armed with this knowledge, you can place the sounds inside a mix with pinpoint accuracy and even change their positions throughout the course of the song using automation.

## Summary
