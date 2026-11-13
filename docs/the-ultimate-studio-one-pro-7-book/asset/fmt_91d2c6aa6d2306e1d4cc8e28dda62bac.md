
# Audio Processing Techniques in Studio One

## Sidechain Compression

If we can find a way to turn the bass down for just those milliseconds, the problem will be solved. This can easily be achieved by a specific version of sidechaining, known as `sidechain compression`. Basically, we will insert a compressor on the bass track, but instead of listening to the bass track, this compressor will listen to the kick track and turn the bass down when it detects a signal on the kick track.

Here’s how to implement sidechain compression based on our example of kick and bass tracks:

1. Open the `Mix` window if it’s not open already.
2. Insert a compressor on the bass channel.
3. Click the `Sidechain` button on top of the compressor interface. This will open a pop-up menu containing the list of all channels active in the project:

![Figure 13.15: The sidechain options for a plug-in](image/B31512_13_15.jpg)

Figure 13.15: The sidechain options for a plug-in

4. From this list, click the `Send` checkbox corresponding to the kick channel.
5. We want the compressor to act as soon as it detects a hit on the kick channel, so next, we set the compressor’s `Attack` parameter to a fairly low value: 4 or 5 milliseconds is a good average setting. Studio One’s compressor can act much faster than that, but such extreme attack values may result in clicking sounds.
6. We also want the compressor to stop compressing as soon as the sharp attack of the kick drum has ended. Set the `Release` time to somewhere around 50 milliseconds, but you can try higher values if you want to create the landmark pumping bass sound used in electronic dance music. There is no clear-cut value here; it depends on the song’s tempo and the temporal characteristics of the sounds, so experiment till you get it just right.
7. You can be a bit more aggressive with the compressor’s `Ratio` and `Threshold` settings since this compression will take place within a time frame of milliseconds. The goal here is to turn the bass down momentarily as much as possible without causing a noticeable drop in its volume.

That’s it! Now, although this compressor is inserted on the bass channel, it listens to the kick channel and responds to the hits of the kick drum. The signal flow looks like this:

![Figure 13.16: The signal flow for sidechain compression](image/B31512_13_16.jpg)

Figure 13.16: The signal flow for sidechain compression

This example is just one way of using sidechain processing in your projects. Sidechain capability is not available on all effects plug-ins, but it’s being added to more plug-ins with each new version. Next time you open a plug-in, check whether it supports sidechaining and think about how you can use that capability to add creative, dynamic effects to your mix.

## Using Splitter for Precise Targeting

`Splitter` is a utility plug-in that lets you split a signal into multiple paths. You can then process each of these paths separately, which opens the door to some versatile and powerful sound processing options. Here are some of the possibilities:

* Split a drum loop into two identical paths and compress one of them. This will effectively give you New York compression within a single channel.
* Split the low-frequency content of a sound into a separate path and convert it to mono. This technique is used to maximize mono compatibility of mixes and can normally be achieved only with high-end plug-ins called `multiband imagers`; however, by using Splitter, you can do this using nothing more than Studio One’s stock plug-ins.
* Split the left and right channels of a stereo track and process them separately. This allows you to come up with some very creative sound design ideas that would not be possible when processing left and right channels with a single plug-in.

Whereas the plug-ins we’ve discussed so far modify the sound going through a channel, Splitter modifies the signal path itself, so using Splitter requires a different method compared to other plug-ins. Let’s walk through the procedure step by step:

1. Drag `Splitter` from the `Browse` window and drop it on the `Inserts` rack of the channel you want to process. This will open the `Routing` window:

![Figure 13.17: The Routing window with Splitter](image/B31512_13_17.jpg)

Figure 13.17: The Routing window with Splitter

1. The left side of the `Routing` window shows the settings for the Splitter we’ve just inserted. Use `Splits` to choose how many paths you’d like to split the signal into. For this example, we will have two paths.
2. Use `Levels` to set the volume for each path and `Mute` to mute paths temporarily.
3. `Split Mode` sets the criteria based on which the signal will be split. There are three options:
    * `Normal` splits the signal into two or more identical copies. This is useful for applying parallel processing within a single track.
    * `Channel Split` splits a stereo channel into pairs of mono signals, allowing you to process left and right channels separately.
    * `Frequency Split` splits the signal into frequency bands and places each band on a separate path. In Figure 13.18, I’m splitting the signal at 240 Hz. You can think of this as a crossover point; any frequency below 240 Hz will be routed to path one and any frequency above that will be routed to path two. If you need to divide frequencies into smaller bands, increase the `Splits` setting to create additional paths and adjust their respective crossover frequencies as needed.

![Figure 13.18: Frequency Split mode on a Splitter](image/B31512_13_18.jpg)

Figure 13.18: Frequency Split mode on a Splitter

1. Once you’ve split your channel, you can drag effects plug-ins from the `Browse` window and drop them on a path you want to process. In Figure 13.19, I have an `Autofilter` plug-in processing the left channel and `Beat Delay` processing the right channel.

![Figure 13.19: Adding plug-ins to Splitter’s paths](image/B31512_13_19.jpg)

Figure 13.19: Adding plug-ins to Splitter’s paths

You can add as many Splitters as you like, create as many Splits within each of them as you like, and insert as many plug-ins on each path as you like. You can also drag every item in the `Routing` window to change its position in the signal flow and create awesome effects chains that are unique to you. Splitter really is an audio engineer’s playground!

## Using Groups for a Faster Workflow

If you have several channels that you want to process together, you can link their parameters using the `Group` function. Actions you perform on any channel within a group will be applied to the other channels as well. This is a great time-saving feature that will help streamline your workflow.

To place channels in a group, proceed as follows:

1. On the `Mix` window, select the channels that you want to group together and right-click on any one of them.
2. Select `Group Selected Tracks` in the pop-up menu. This will open a dialog box where you can name the group.
3. Give the group a short, descriptive name and click `OK`. This will help you identify groups easily when you’re working on a large project.

Channels in a group will be marked by a group icon on top of their faders:

![Figure 13.20: Group icons indicating grouped channels](image/B31512_13_20.jpg)

Figure 13.20: Group icons indicating grouped channels

By default, placing channels in a group links their faders, pan controls, and solo, mute, record enable, and monitor buttons, as well as send routings. To exclude any of these parameters from the group, do the following:

1. Click the `Show Groups` button on the `Mix` window. This will open the `Group Panel`:

![Figure 13.21: The Show Groups button and Groups panel of the Mix window](image/B31512_13_21.jpg)

Figure 13.21: The Show Groups button and Groups panel of the Mix window

1. Right-click on the name of the group you’d like to modify.
