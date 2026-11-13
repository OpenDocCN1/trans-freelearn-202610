
## Pattern Sequencer

![Figure 9.21: The Pattern Sequencer inside Arpeggiator](image/B31512_09_21.jpg)

Figure 9.21: The Pattern Sequencer inside Arpeggiator

These columns represent the steps triggered by `Arpeggiator`. Drag a column up or down to adjust its velocity. Drag a column left or right to adjust its width (acting as a gate setting for individual notes).

Drag the `Pattern Length` slider at the bottom if you want to program a pattern with more than eight steps. The maximum number of available steps is 32.

Using `Arpeggiator` in this manner opens the door to many creative possibilities, but this is just the tip of the iceberg. You can automate every parameter on `Arpeggiator` to change your patterns dynamically throughout a project. You can even assign several arpeggiators on a track and go back and forth between them using automation. We will talk about automation in [Chapter 13](#B31512_13.xhtml#_idTextAnchor218).

## Chorder

`Chorder` is an intelligent chord generator that lets you trigger chords by playing single notes. This allows you to play complex chords easily even if you have limited keyboard skills.

`Chorder`’s interface consists of two rows of piano keys:

![Figure 9.22: Chorder](image/B31512_09_22.jpg)

Figure 9.22: Chorder

The keyboard at the bottom shows the note that you play on your keyboard. The keyboard at the top shows the notes that will be triggered when you play that note. The slider above the keyboards sets the range within which `Chorder` will operate. If you play any note outside of this range, `Chorder` will ignore it.

If you’re new to music theory and don’t know how to construct chords, you can still get a lot of mileage out of `Chorder`. In the `Preset` menu, you’ll find a good collection of chord combinations optimized for various music genres. Open the `Preset` drop-down menu (at the top of the interface, look for the field labeled `default`), select a chord combination that matches your style, and you’re good to go. However, if you want to program your own chords to use in `Chorder`, then read on.

### Programming Chorder to use custom chords

You can teach `Chorder` to trigger as many custom chords as you want. As an example, we will program it to trigger a C Maj7 chord when we play the `C2` note on our MIDI keyboard. To do so, follow these steps:

1. Click the `Learn Mode` button.
2. On the lower keyboard, click on the note that you want to use as a trigger. In our example, that will be `C2`.
3. On the upper keyboard, click the notes that you want `Chorder` to trigger when you play `C2`. In our example, these will be `C`, `E`, `G`, and `B`. Feel free to arrange these notes any way you like to create different chord voicings, add tension notes such as `D`, `F#`, and `A`, and use different octaves. You can use any note in any octave on the upper keyboard.
4. Instead of clicking with your mouse on the upper keyboard, you can add notes by playing them on your MIDI keyboard as well. `Chorder` will register them in the same manner.
5. If you add a wrong note, click on it or play it on your keyboard again to remove it.
6. To enter another chord, repeat steps 2 to 4 as many times as necessary.
7. Click the `Learn Mode` button again to exit Learn Mode.

Now, you only need to record the trigger notes on the track. When you play back the recording, `Chorder` will automatically generate the chords that you have programmed.

## Repeater

`Repeater` is a simplified version of `Arpeggiator`. Instead of arpeggiating the notes that it receives, it simply repeats them at predetermined intervals. This is useful for creating rhythmic sequences from long, static notes.

Most of the settings on `Repeater`’s interface are identical to those on `Arpeggiator`:

![Figure 9.23: Repeater](image/B31512_09_23.jpg)

Figure 9.23: Repeater

To avoid repetition (no pun intended), we will cover only the settings unique to `Repeater` here.

`Steps` determines how many times the original note will be repeated. The default is `8`, but it can go as high as `32`. Once this number of repetitions is completed, `Repeater` will stop.

The `Scale` settings for `Velocity`, `Gate`, and `Pitch` allow you to change these values gradually. For example, by adjusting the `Scale` setting for `Velocity`, you can tell `Repeater` to increase or decrease the velocity of each subsequent step compared to the previous one.

The velocities and lengths of all steps are controlled globally by the `Velocity` and `Gate` settings, respectively. You can override these settings and set specific velocity and length values for individual steps by activating the individual `Velocity` and `Pitch` buttons in the top-right corner. When these are active, you will be able to drag the columns in the sequencer window to adjust their length and velocity, just like in `Arpeggiator`.

## Input Filter

`Input Filter` is a utility plug-in that lets you filter out unwanted notes before they reach your MIDI device. This is useful in situations where you want an instrument to respond to only specific notes, leading to some creative applications.

Let’s take a look at how `Input Filter` works. The interface is simple:

![Figure 9.24: Input Filter](image/B31512_09_24.jpg)

Figure 9.24: Input Filter

The notes within the key range will be allowed to pass, and notes outside the range will be stopped. `Input Filter`, as shown in Figure 9.24, is set to pass all notes between `C2` and `C4`, stopping all other notes. You can edit this range by typing in note names directly inside the boxes on both sides of the `Key Range` field, or by using the slider above the keyboard.

`Input Filter` also allows you to set a velocity range for notes that will be allowed to pass. Remember that in Studio One, MIDI notes can have a velocity value between `0%` and `100%`. You can set a lower and upper threshold for velocity using the `Min. Velocity` and `Max. Velocity` fields respectively. Any notes above or below these thresholds will be stopped.

Now, let’s think of a couple of scenarios where these features would be useful. For example, let’s say you want to split your keyboard between two instruments so that when you play in lower octaves, instrument A will respond with a bass guitar sound, and when you play in higher octaves, instrument B will respond with a piano sound. You can do this as follows:

1. Create two Instrument Tracks, one for instrument A and one for instrument B.
2. Select the same MIDI keyboard as the input source for both tracks.
3. Load an `Input Filter` on both tracks.
4. On the `Input Filter` for the instrument A track (bass guitar), set the allowed range of notes between `C0` and `C2`.
5. On the `Input Filter` for the instrument B track (piano), set the allowed range of notes between `C2` and `C8`.

Here’s another scenario. Let’s say you’re working on an orchestral arrangement. Instrument A gives you the sounds of a string section, and you want instrument B to come in and play timpani sounds only when you play hard to accent certain notes. Here’s how to set it up:

1. Create two Instrument Tracks, one for instrument A and one for instrument B.
2. Select the same MIDI keyboard as the input source for both tracks.
3. Load an `Input Filter` on both tracks.
4. On the `Input Filter` for the instrument A track (strings), set the allowed range of velocity between `0%` and `100%`.
5. On the `Input Filter` for the instrument B track (timpani), set the minimum velocity threshold to `75%`.

Now, you will hear the timpani only if you play at a velocity higher than `75%`.
