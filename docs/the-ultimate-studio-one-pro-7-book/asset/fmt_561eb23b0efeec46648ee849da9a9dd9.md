
# Chapter 8: Melodyne and Stem Separation

Melodyne’s interface is intuitive and easy to understand, but learning to interpret the blobs takes some getting used to. Feel free to experiment. If you’re not happy with an action you performed, go to Melodyne’s own menu and then click `Edit` and `Undo`. Don’t use the keyboard shortcut to undo, as this will undo the last action you took in Studio One’s `Arrangement` window!

Operations performed in Melodyne are non-destructive. Back in the `Arrangement` window, you can always right-click on the event and select `Remove Melodyne`. This will restore a recording to its original state.

## Important Note

Although we have talked about vocal recordings, Melodyne can just as easily work on any monophonic source material – that means you can use it to correct the pitch of any instrument, as long as it’s playing just one note at any given time and not playing chords. Try it on any instrument that can go out of tune. It works great on string instruments and wind instruments.

So far, we have talked about using Melodyne for corrective purposes. Up next, we will explore a more creative method of working with Melodyne to enhance your projects.

## Using Melodyne to Create Background Harmony Vocals

Background harmony vocals definitely take a song to the next level. However, creating and singing harmony vocals takes a lot of experience and requires at least a basic knowledge of music theory, so it may not be possible to record them live. Using Melodyne, we can create harmony vocals from a singer’s original performance. Here’s how:

1. During your recording session, record a few extra takes of your singer’s performance. Keep the best take for the lead vocal. We will use the other takes to create background harmony vocals.
2. Right-click on one of these extra takes and select `Edit with Melodyne`.
3. In Melodyne’s window, click on the `Chord Track Visibility` button:

![Figure 8.18: The Chord Track Visibility button in the Melodyne window](`image/B31512_08_18.jpg`)

4. Type in the chords of your song on the chord track.
5. On Melodyne’s own top menu, go to `Options` | `Pitch Grid` | `Pitch Background`, and select `Chord`. Now, the background of the Melodyne window will display the notes of the chords in white and all other notes in gray.
6. Again, on Melodyne’s own top menu, go to `Options` | `Pitch Grid`, and select `Chord Snap`. Now, the blobs can only be moved to the notes of the chords used in your song.
7. Drag the blobs to these notes to create harmonious vocals. Since the blobs snap to your chord tones only, there is no chance of coming up with a wrong note. Feel free to experiment.
8. Repeat the previous steps to create as many harmony vocal tracks as you want.

Remember that Melodyne can be used with instruments as well, so you can use this method to create a violin section from a single violin performance, for example. The only limit is your imagination!

This concludes our guided tour of corrective and creative ways to use Melodyne in your projects.

## Using Stem Separation to Isolate Audio Events

**Stem separation** refers to the process of isolating different elements or **stems** of a mixed audio file, such as vocals, drums, bass, and other instruments, into individual audio tracks. This allows for more possibilities in remixing, re-arranging, or enhancing the components of the original mix, offering creative flexibility.

Stem separation has been available for quite some time now, but it was commonly found on websites operated by third-party vendors, and the typical workflow for extracting stems involved uploading the original file, paying and waiting for the extraction, and then downloading the extracted files.

As of version 7, Studio One has stem separation capability built right into its feature set. Here’s how it works:

1. Drag the audio file you’d like to extract stems from and drop it into your Studio One Song.
2. Select the audio file by clicking on it with the `Arrow` tool.
3. Press `Cmd` + `U` (Mac) or `Ctrl` + `U` (Windows) to open the `Separate Stems` dialog box (Figure 8.19).

![Figure 8.19: The Separate Stems dialog box](`image/B31512_08_19.jpg`)

4. Here, select the audio material that you’d like to extract from the original recording. You can select `Vocals`, `Drums`, and `Bass`, which are self-explanatory, and `Other`, which is basically every other instrument that does not fall under the first three categories.
5. Click `OK`.

Studio One will use its AI magic to analyze the recording and extract the stems you have selected into separate, individual tracks. From there, you can work with these stems to sample, remix, and rearrange music to your heart’s content. If you’re planning to release the fruit of this labor on digital platforms, though, make sure that you are not violating any copyright regulations!

## Summary

In this chapter, we explored Studio One’s audio editing tools and discussed several different methods to use these tools in your projects. We started with common editing tasks, such as cleaning up an audio track, and then moved on to more complex tasks, such as conforming one track to the groove of another. We continued with an in-depth discussion of how you can incorporate Melodyne into your projects to enhance vocal performances. We concluded by exploring how to use stem separation technology to extract material from already mixed music files.

In the next chapter, we will explore Studio One’s MIDI editing tools and discover how they can help bring your virtual instruments to life.

# Chapter 9: Editing MIDI

Working with MIDI is like playing an instrument. It’s a mixture of art and science, and you cannot just focus on one while ignoring the other. Just like playing an instrument, working with MIDI requires practice. Whether you’re new to music production or a seasoned pro who’s switched to Studio One, this new instrument will offer powerful tools to create and shape your musical ideas while retaining a friendly learning curve.

In this chapter, we will cover Studio One’s extensive set of MIDI editing tools using a step-by-step approach, starting with the basic ones and proceeding to the more advanced ones. By the end of this chapter, you will be able to use these tools confidently to adjust the pitch, timing, and dynamics of MIDI performances. This will allow you to achieve a wide range of musical results, from correcting simple mistakes to creating complex and layered musical arrangements.

We will cover the following topics:

* Using basic editing tools and quantization
* Adjusting MIDI data to enhance a performance
* Working in the Pattern Editor
* Using Note FX to spark creativity

## Technical Requirements

To follow along with this chapter, you need to have Studio One installed and activated on your computer.

## Using Basic Editing Tools and Quantization

In [Chapter 6](#B31512_06.xhtml#_idTextAnchor082), we covered several different methods of recording and programming a MIDI performance. While a MIDI performance is an excellent starting point to present musical ideas, it’s hardly ever used in its original form. It is meant to be processed and refined, just like a precious metal ore.

In this section, we will explore Studio One’s basic MIDI editing tools and then take a deep dive into MIDI quantization. All the tools and methods discussed in this section will be based on the `Edit` window, which can be easily accessed by double-clicking on the MIDI event that you’d like to work on.
