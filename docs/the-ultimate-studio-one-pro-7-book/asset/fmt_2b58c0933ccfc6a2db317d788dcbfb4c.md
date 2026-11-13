
*   `Swing` defines the time relationship between pairs of steps. When it’s at `0`, its default setting, each step in a pair is of equal duration. As you increase the swing value, the first note in a pair will become longer and the second will become shorter, resulting in a “swing” feel.

*   `Gate` sets the length of each step. At its default value of `100%`, every step is played at its full length. Reducing this value will make the notes shorter and increasing it will make them longer.

*   `Accent` sets how much emphasis is placed on accented notes. You can set any note as an accented note by pressing `Ctrl` (Windows) or `Cmd` (Mac) and clicking on it.

You’ve created the perfect pattern, but using it over and over through the entire song will become monotonous. You need a way to create variations of your pattern and sprinkle them throughout your song to make it dynamic and exciting. Let’s do it:

1.  Click the Pattern Inspector button on the top-left corner of the `Edit` window. This will open the Pattern Inspector panel:

![Figure 6.20: The Pattern Inspector panel with the Panel Inspector button highlighted](image/B31512_06_20.jpg)

Figure 6.20: The Pattern Inspector panel with the Panel Inspector button highlighted

1.  The pattern you’re currently working on is listed here with the default name `Intro`.

2.  Click the plus-shaped New Variation button to create an empty pattern or the Duplicate Variation button to create a duplicate copy of your existing pattern.

3.  Select the newly created pattern by clicking on it. The Pattern Editor will now display whatever is inside the new pattern.

4.  Work on the new pattern to create a variation to be used in a different section of the song.

5.  Create as many variations as you need.

6.  Double-click on the names of these variations to give them descriptive names.

7.  Once you’ve created your arsenal of patterns, you can drag them from the Pattern Inspector panel and drop them directly on the `Arrangement` window.

This wraps up our coverage of Studio One’s extensive MIDI inputting methods. Up next, we will take a look at a much more sophisticated way of creating MIDI data: converting audio recordings to MIDI.

# Using Melodyne to convert audio to MIDI

Until recently, converting audio recordings to MIDI data was pure science fiction. But today, any DAW software worthy of its name boasts this function in its feature list. Studio One comes bundled with `Melodyne`, the industry standard in audio pitch detection and correction, so it has the upper hand compared to its competitors. Be warned, though, that this technology is still in its infancy. The success of conversion depends, to a large extent, on the source material.

Let’s take a look at the things you can do to get the best results possible.

## Choosing the right type of audio material

The first and most critical step in audio-to-MIDI conversion is to identify the pitches or notes in an audio recording. This task is performed by a pitch detection algorithm. The success of this algorithm depends heavily on two factors:

*   Pitch detection works best with monophonic material. The term *monophonic* has different meanings in different contexts, so let’s clarify: **Monophonic** audio material comprises one single melodic line only (that means no chords or any harmonic material, nor any accompaniments). So, for example, a guitar playing single notes is a monophonic source, but a guitar playing chords is not.

*   Pitch detection algorithms yield the best results with material that allows them to clearly establish where a note begins. In practical terms, this means that legato material, where notes are connected, is more challenging compared to staccato material, where notes are detached. Also, if the performance in the audio recording has lots of sliding in and out of notes (glissandos), the algorithm will have a hard time determining the pitches and starting points of notes.

Even if your audio material does not meet the preceding criteria, there are a few tricks you can try to get mileage out of it. We’ll cover those in the next section.

## Audio-to-MIDI-conversion walkthrough

Once you have created a new Song, follow these steps to convert an audio recording to MIDI data:

1.  Add the virtual instrument you’d like to use.

2.  Next, load a sound into this instrument using its `Preset` menu.

3.  Locate the audio file you’d like to convert to MIDI. Drag it from `Explorer` (Windows) or `Finder` (Mac) and drop it into the Song. Studio One will create an audio track and place the audio recording as an audio event inside the track.

4.  Right-click on this audio event. From this menu, go to `Audio`, and then click on `Edit with Melodyne`. This will send the audio material to Melodyne, which will open in the `Edit` window:

![Figure 6.21: Melodyne with the Correct Pitch Macro button highlighted](image/B31512_06_21.jpg)

Figure 6.21: Melodyne with the Correct Pitch Macro button highlighted

Note that Melodyne has detected all the pitches in the audio recording and it’s displaying them in a layout that resembles Studio One’s `Piano view`, which we talked about earlier in this chapter. We will take an in-depth look at Melodyne in [*Chapter 8*](#B31512_08.xhtml#_idTextAnchor121). For now, we will only perform a simple operation to make sure that there are no out-of-tune notes that may confuse the pitch detection algorithm.

1.  Press `Ctrl` + `A` (Windows) or `Cmd` + `A` (Mac) to select all the notes.

2.  Click the `Correct Pitch Macro` button. This will open a small dialog box:

![Figure 6.22: Melodyne’s Correct Pitch Macro dialog box](image/B31512_06_22.jpg)

Figure 6.22: Melodyne’s Correct Pitch Macro dialog box

1.  Pull the `Pitch Center` and `Pitch Drift` sliders on this dialog box all the way up to `100 %`. Click `OK`.

    This is a rather crude way of fixing out-of-tune notes, but it’s perfectly adequate for our purpose here.

2.  Close the `Edit` window. Back on the `Arrangement` window, you will see that the audio event now has some colorful dots overlaid on top of the audio waveform. These are the MIDI notes that have just been created.

3.  You can now drag the audio event and drop it directly on the track you created for your virtual instrument. Studio One will copy the MIDI data over to the `Instrument Track`.

4.  Play the project and see how the conversion went. Some manual tweaking will most likely be necessary. Refer to [*Chapter 9*](#B31512_09.xhtml#_idTextAnchor139) to edit the converted notes to perfection.

Converting audio to MIDI is an exciting feature that opens unlimited possibilities. You can, for example, use this procedure to convert a percussion recording to create your own rhythmic groove. It will only get better as algorithms improve.

# Summary

In this chapter, we took an in-depth look at Studio One’s several methods for recording and inputting MIDI data for use with virtual instruments. We covered a variety of music production scenarios and discussed which method would be the best choice for a given workflow, including recording notes live, painting them with a mouse, or programming them with a sequencer.

So far, we focused on creating and pouring our musical ideas into our projects. In the next part of this book, we will talk about how to rework and improve those ideas by using Studio One’s excellent editing tools. We will start by taking an in-depth look at the global editing tools in the next chapter and lay the groundwork for the editing best practices that we will be using throughout this book.

# Part 3: Editing in Studio One
