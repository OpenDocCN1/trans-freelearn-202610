
# Customizing plug-ins

As we saw in [*Chapter 12*](#B31512_12.xhtml#_idTextAnchor195), there is a huge selection of effects plug-ins, and there are infinite ways to use them, but as you develop your mixing abilities, you’ll find yourself building a short list of tried and trusted plug-ins and using them more than the others. You will also find that you set those plug-ins pretty much the same way across your projects. This is not a bad thing; on the contrary, it’s part of your unique production style.

Studio One makes it easy for you to arrange your favorite plug-ins and launch them in your projects just as you want to see them, saving you time and streamlining your workflow. Let’s explore the features that make this possible.

## Adding to Favorites

If you find yourself using a plug-in quite often, consider adding it to the `Favorites` category in the list of plug-ins:

![Figure 13.22: A list of plug-ins on the Browse panel](image/B31512_13_22.jpg)

Figure 13.22: A list of plug-ins on the Browse panel

This will make the plug-in easy to find, especially if you have an extensive set of plug-ins. Here’s how to do it:

1.  Open the `Browse` panel and go to the `Effects` tab.
2.  In the list of plug-ins, right-click on the plug-in you want to designate as a favorite.
3.  On the pop-up menu, select `Favorite`.

`Favorites` will appear on the top of all the plug-in lists – on the `Inserts` racks of the `Mix` window, on the `Inspector` panel, and in the `Browse` window. Your favorite plug-ins will always be just a click away.

## Creating FX chains

If you find yourself using the same plug-ins, with the same settings and arranged in the same order, you can save them in an FX chain and instantly recall them in your projects.

For example, I like using `Room Reverb`, but since it does not have a built-in equalizer section or filters, I add `ProEQ3` after the reverb to get rid of the extreme frequencies. Then, I add `Binaural Pan` to add some extra width to the reverb for a more three-dimensional mix. This has been my go-to reverb setup for quite some time now.

Instead of adding these plug-ins one by one and tweaking them from scratch on every project, I can create an FX chain for them and recall it in my future projects. Here’s how to do it:

1.  Arrange your plug-ins on a channel’s `Inserts` rack.
2.  Open each plug-in and tweak it just as you want to see it on your next project. Studio One will remember your settings for each plug-in as well.
3.  Click the small triangle at the top of the `Inserts` rack. This will open the `FX Chain` menu:

![Figure 13.23: The FX Chain menu](image/B31512_13_23.jpg)

Figure 13.23: The FX Chain menu

4.  On this menu, click `Store FX Chain`. This will open a dialog box, asking you to name your new FX chain.
5.  Give a short, descriptive name to your FX chain (such as `main vocal reverb`) and click `OK`.

Next time you open the `FX Chain` menu from any channel on any project, this FX chain will be waiting for you to launch it with exactly the same settings. What a time saver!

## Adding presets to plug-ins

If you frequently use the same settings on a plug-in, you can save yourself the trouble of adjusting them from scratch on every project by making them a preset. Follow these steps to do so:

1.  Set all the parameters of the plug-in the way you want to store them in the preset.
2.  Click the tiny page icon on the plug-in’s interface to open its presets menu:

![Figure 13.24: The plug-in preset menu](image/B31512_13_24.jpg)

Figure 13.24: The plug-in preset menu

3.  Click `Store Preset`, give a descriptive name to the preset (such as `lead guitar delay`), and click `OK`.

Now, this preset will be available every time you launch this plug-in on any channel and in every project.

This concludes our tour of advanced mixing techniques and workflow hacks within Studio One’s Mix Console. Next, we will talk about how to export final mixes or stems from your Songs so that you can publish them or share them with collaborators.

# Exporting final mixes and stems

Once you’re happy with your mixes, you’ll want to share them with the rest of the world. However, since the rest of the world cannot open our Studio One project files, you will first need to convert your mixes to a standard file format that everyone can open in their media players. This process is called **exporting**. When you export a `Song` in Studio One, the multitude of channels in your project will be bounced down to a single stereo channel, which will then be saved in a globally recognized audio file format.

There are two methods of exporting a `Song` from Studio One, which are used for different purposes. Let’s start with the more commonly used method.

## Exporting final mixes

Exporting a final mix from a `Song` means bouncing all the channels into one stereo channel and embedding that stereo channel in an audio file. It sounds complicated, but Studio One handles the task gracefully once you set a few parameters.

Follow these steps in order to export the final mix from your `Song`:

1.  Set the left and right locators in the `Arrange` window to mark the beginning and end of the `Song`, respectively. By default, Studio One will export only the part of the timeline that falls between these two locators, so take your time and position them correctly.

> **Important note**
>
> It’s a good idea to leave a tiny bit of silence at the beginning of a song (about half a second will do). This will prevent consumer-level media players from chopping off the beginning of the song.
>
> At the end of the song, make sure that all reverb tails have decayed completely before the right locator so that they don’t cut off abruptly when played back.

2.  Go to the top menu, click `Song`, and then `Export Mixdown`. This will open the `Export Mixdown` window:

![Figure 13.25: The Export Mixdown window](image/B31512_13_25.jpg)

Figure 13.25: The Export Mixdown window

3.  In the `Location` section of this window, select a folder and name for the audio file(s) that you’re about to create.
4.  In the `Format` section, select the format of the files you want to create. You can use the checkboxes to select multiple file types. Click on each file type to access its applicable parameters. While there are several file type choices, here are the two that you’re likely to use the most:

    *   **Wave File** is the industry standard and is also universally recognized by consumer-grade media players. Wave files offer high sound quality at the price of large file sizes. Use this format to deliver final mixes to distribution networks, clients, or mastering studios. Here are the two most common parameters for wave files:

        *   If you export a song that will be distributed to music platforms such as Apple Music or Spotify, set `Resolution` to `16 Bit` and `Sample Rate` to `44.1 kHz`. This is known as the `CD standard`. It’s been the standard format for music distribution since the 1980s and is still used by music-only platforms.
