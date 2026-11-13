
So, we use a method that gives us the best of both worlds: we can go crazy with effects that we feed into our performer’s headphones while recording only the dry, unprocessed signal. Here’s how it works:

![Figure 5.4: Signal flow to and from an audio track](image/B31512_05_04.jpg)

Figure 5.4: Signal flow to and from an audio track

Let’s assume that you want to record your own vocal performance for a song you’re working on. You connect your microphone to `Input L` on your audio device; the signal from your microphone is now reaching Studio One through the strip labeled `Input L` on the mixer console (*a* in *Figure 5.4*). Then, you create a new audio track and select `Input L` as the input source (*b* in *Figure 5.4*). Studio One automatically creates a new channel strip on the mixer console to represent the output coming from this new track (*c* in *Figure 5.4*).

A track is like a tape recorder. It’s where the audio signal is recorded and played back. If we substitute the track in *Figure 5.4* (*b*) with the image of a tape recorder, the signal flow will be much easier to visualize:

![Figure 5.5: Another way of looking at signal flow](image/B31512_05_05.jpg)

Figure 5.5: Another way of looking at signal flow

The signal coming from your audio device will first hit the `Input` channel (*a* in *Figure 5.5*) and then be recorded on the track (*b* in *Figure 5.5*). It will then be routed to the channel strip for the track (*c* in *Figure 5.5*).

You will notice that both of these mixer channels (*a* and *c* in *Figure 5.5*) have a section titled `Inserts`. `Inserts` are virtual slots on Studio One’s mix console that we use in order to apply sound effects. So, for example, if you want to add reverb to give a sense of depth and ambiance to your vocals, you could apply a reverb plug-in on either of these `Inserts` sections and the end result would sound exactly the same.

The difference, however, is that any effect you insert on the input channel will be recorded to the track. Going back to our example, if you insert a reverb plug-in on the input channel, your vocal performance will be recorded with the reverb baked into it. You will not be able to remove it or modify it when you start mixing your song.

The solution is to apply effects on the `Inserts` section of the channel strip for the track. You can go ahead and try all sorts of effects here, without having to commit to them. Studio One will only record the dry, unprocessed signal coming from the input channel.

Here are some suggestions for effects plug-ins that you may try on your next recording session:

- Adding a `Compressor` plug-in with a gentle setting (with the `Ratio` parameter set to anywhere between 2 and 4) allows singers to hear themselves better in their headphones and helps them deliver a better performance.
- Everyone loves reverb in their own voice. Insert Studio One’s own `Room Reverb` plug-in, select a `Large Hall` setting, and adjust the `Mix` parameter to taste.
- Use Studio One’s `Tuner` plug-in to make sure the instruments you’re recording are tuned correctly. Remember that the signal flows from top to bottom in the `Inserts` section of a channel strip; if you have several plug-ins inserted, the signal will first hit the plug-in at the top of the list and then travel toward the bottom. The `Tuner` plug-in may become confused by effects added by other plug-ins, so drag it to the top of the list.
- Use `Ampire` to add color and character to otherwise dull and dry sound sources. Although designed primarily for electric guitars, it works well on almost every instrument. This added tonal “attitude” will help you get into the right mood and deliver a better performance.
- Try `Redlight` to add some extra bite and brightness to dull vocals.

Great! By applying these effects, you have placed your performers in the best light, and everyone is excited to start recording. There is one final step left before you can actually hit the `Record` button.

## Setting up the metronome

In order to reap the benefits of Studio One’s stellar editing features, which we will cover in the following chapters, I strongly suggest that you record with Studio One’s built-in metronome. If you prefer not to use a metronome at all (e.g., you’re recording a classical piece with a free tempo), feel free to skip this section.

You can turn the metronome on or off by hitting `C` on your keyboard or by clicking its icon in the lower panel:

![Figure 5.6: Precount, Metronome Setup, and Metronome on/off](image/B31512_05_06.jpg)

Figure 5.6: Precount, Metronome Setup, and Metronome on/off

If you activate `Precount`, when you hit the `Record` button, the metronome will count for one bar before the recording actually starts. This will give you time to get your instrument in position, take a breath, and get ready for recording (it’s an invaluable feature, especially if you’re recording by yourself).

For the most common recording scenarios, this will be all you need to know about using the metronome, but if you need to tweak it further, just click on the wrench-shaped `Metronome Setup` icon and you will be taken to this page:

![Figure 5.7: The Metronome Setup window](image/B31512_05_07.jpg)

Figure 5.7: The Metronome Setup window

In this menu, you can set the metronome using three different sounds: `Accent` plays on the first beat of each bar and `Beat` plays on all other beats. So if your time signature is set to `4/4`, `Accent` will play on the “1” of each bar and `Beat` will play on beats 2, 3, and 4.

`Offbeat`, which is turned off by default, will play halfway between each beat. You know how a 4/4 measure is sometimes counted as “*ONE AND TWO AND THREE AND FOUR AND…*”? `Offbeat` is responsible for sounding those *ANDs*. This could be useful if you’re working at a very slow tempo and need to hear the metronome with smaller subdivisions.

This menu allows you to set the respective volumes of each of these three metronome sounds. You can even change the default sounds if they clash with other sounds in your arrangement.

Moving on down the window, `Precount`, as we mentioned, tells Studio One to count one measure before actually starting to record. `Preroll` is also designed to help you get ready for recording, but it’s more useful when you want to start recording somewhere in the middle of the song. With `Preroll` enabled, when you hit `Record`, Studio One will go back one bar before the current position of the timeline, start playback, and then automatically start recording once the playback passes the cursor position.

You can change the `Precount` and `Preroll` times here if you need more time to get ready to perform.

`Repeat Accent` makes the metronome easier to follow if you’re using a time signature with more than one accent per bar. For example, if your time signature is `12/8` and `Repeat Accent` is disabled, the metronome will put an accent on the first beat of the bar only. If you enable it, the metronome will place an accent on every fourth beat (ONE two three FOUR five six…) making it much easier to follow.

If you activate `Click in Precount Only`, the metronome will go quiet after the precount and will stay silent during the actual recording. This is useful if the metronome sound is leaking from headphones and reaching the microphone.

By default, the metronome only clicks when you’re recording. `Click in Play` allows the metronome to be heard during playback as well – perfect for practicing the part before hitting the `Record` button.

Speaking of which, we are finally ready to hit the `Record` button! In the next section, we will talk about the various recording modes available in Studio One and how you can use them to capture the best performance in a variety of scenarios.

## Understanding different recording modes
