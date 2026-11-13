
4. If you don’t hear anything, make sure that the input selector (`c` in *Figure 6.10*) points to the keyboard you’re using and that the output selector (`d` in *Figure 6.10*) points to the instrument associated with this track.

5. Recording with a metronome will make your life much easier when you want to add other instruments or edit your recording. Activate the metronome’s click sound by pressing `C` on your keyboard. If you need Studio One to count in before starting to record, set the `Metronome`, `Preroll`, and `Precount` options as described in [*Chapter 5*](#B31512_05.xhtml#_idTextAnchor065).

6. Activate recording by clicking the `Record` button or by pressing `*` on your numeric keypad.

7. When you’re done, click the `stop` button or press the `spacebar`.

If you’re happy with your recording but there is a spot that you’d like to record again, you can re-record that part only, while leaving everything else intact, by using the `Auto Punch` feature described in [*Chapter 5*](#B31512_05.xhtml#_idTextAnchor065).

This is all you need to do to record a live performance with a MIDI or QWERTY keyboard. Don’t worry if there are a few mistakes in the performance that need to be fixed; you will be able to edit your performance to perfection using Studio One’s powerful editing tools, which we will cover in [*Chapter 7*](#B31512_07.xhtml#_idTextAnchor099).

### Loop recording

**Loop recording** is a convenient feature that lets you loop a section of your song and allows Studio One to record on that section indefinitely until you hit stop. This saves you from constantly having to manually start recording, stopping, rewinding, and start recording again. It also allows you to focus on singing or playing your instrument.

There are two different loop recording modes when working with MIDI instruments, and they are designed for two distinct purposes: `Record Takes` and `Record Mix`.

Go to `View` on the top menu and select `Record Panel` to access these modes:

![Figure 6.11: The instrument loop record panel](image/B31512_06_11.jpg)

*Figure 6.11: The instrument loop record panel*

With `Record Takes` enabled, every time the timeline reaches the end of the loop, it will jump back to the beginning. Studio One will create a new layer inside the same track and record your performance as a new take on that layer. By the time you hit the stop button, you will end up with as many takes as you allowed the timeline to loop within the section.

This is excellent for trying out new ideas. When you’re done recording, you can pick your favorite parts from all the different takes and create a perfectly edited composite take. You can also use this feature to focus on playing a difficult passage until you nail it.

With `Record Mix`, instead of creating a new layer, Studio One will keep recording on the same layer until you hit stop. This is useful when you want to build a track gradually. For example, when creating a drum groove, you may want to play the kick drum and snare in the first cycle of the loop, then add hi-hat in the second cycle, additional percussion elements in subsequent cycles, and so on. If you play something you don’t like, hit stop, click `Undo Last Loop` (also shown in *Figure 6.11*), and start again.

Once you’ve selected the loop recording mode you’d like to use, follow these steps to start recording:

1. Set the left and right locators to the beginning and end of the section of your song that you’d like to work on.

2. Click the `Loop` button to enable looping.

3. Set `Metronome`, `Preroll`, and `Precount` (see [*Chapter 5*](#B31512_05.xhtml#_idTextAnchor065)).

4. Activate recording by clicking the `Record` button or pressing `*` on your numeric keypad.

5. When you’re done, click the `stop` button or press the `spacebar`.

So far in this section, we have covered the methods you can use to record a live performance using a MIDI or QWERTY keyboard. Now, let’s take a look at other methods of inputting MIDI data to use with virtual instruments.

## Inputting MIDI notes manually

One of the coolest things about MIDI is that it allows you to program a performance as opposed to actually playing it. This opens the door to an amazing range of possibilities. You can use MIDI programming for something as ambitious as constructing an entire orchestral performance using nothing but a mouse, or something as simple as performing a musical idea that is beyond your keyboard-playing skills.

Studio One offers a MIDI programming method to fit every scenario. You can draw MIDI notes using a mouse, play them as slowly as you want without having to chase a metronome, or use a sequencer to craft detailed patterns that can be used over and over in a song. In this section, we will cover each of these in detail.

### Drawing notes with the Paint tool

The simplest way of inputting MIDI notes is to use your mouse or touchpad. Once you’ve created a new Song and added an instrument, follow these steps to enter notes with this basic method:

1. Double-click on the track lane of the Instrument Track in the `Arrangement` window. This will create an empty instrument part:

![Figure 6.12: An empty instrument part next to the Instrument Track](image/B31512_06_12.jpg)

*Figure 6.12: An empty instrument part next to the Instrument Track*

1. Now, double-click on this new instrument part. It will open in the `Edit` window.

2. The `Edit` window has three different view options:

    * The `Piano` view lets you view MIDI data on a conventional, piano-roll-style display, much like you would find on any other DAW program. This is the view that we will be focusing on, as it offers more options for programming and editing.

    * The `Drum` view is optimized for programming drum parts. It displays only the starting points of MIDI notes, and for each MIDI note, it displays the name of the corresponding part of a drum kit, such as kick, snare, hi-hat, and so on. This is an archaic way of working with MIDI, and I would advise against using this view. We will talk about a much more efficient and fun way of drum programming in the *Creating patterns* section.

    * The `Score` view allows you to input notes using musical notation, laid out on a music staff.

    These three views are essentially three different ways of looking at the same MIDI data. Go ahead and select the view that fits your workflow. The steps we cover in this section will work equally on all three of them:

![Figure 6.13: The view selector in the Edit window](image/B31512_06_13.jpg)

*Figure 6.13: The view selector in the Edit window*

1. With the `Edit` window in focus (you’ll know it’s in focus when there’s a blue frame around it), press `3` on your keyboard to select the `Paint tool`. You can also activate it with your mouse if you like, but using keyboard shortcuts will let you work much faster.

![Figure 6.14: The tool selector in the Edit window with the Paint tool selected](image/B31512_06_14.jpg)

*Figure 6.14: The tool selector in the Edit window with the Paint tool selected*

1. Make sure that the `Snap` function is active (highlighted in *Figure 6.15*). This will ensure that the MIDI notes you will be entering conform to the musical time base of your project. Disable `Snap` only if you’re working on a song with a free tempo or if you intentionally want to place some notes outside of the musical grid.

2. Now, open the `Quantize` pull-down menu, also shown in *Figure 6.15*. This menu lists the most common note lengths used in music notation (`T` in this menu indicates triplet notes and `sw` indicates notes with swing). Select the length for the notes that you will be entering. For example, if you select sixteenth notes here, every note you enter using the `Paint tool` will snap to the nearest sixteenth note slot on the `Edit` window grid and the length of each note will be confined to a sixteenth note.
