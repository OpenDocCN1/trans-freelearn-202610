
- The **Producer Edition** includes all features in the Fruity Edition, plus audio recording and post-production tools. If you want to record audio, you will need this version or higher. This version also includes the Edison, Slicex, Sytrus, Maximus, Vocodex, and SynthMaker plugins.
- The **Signature Bundle** includes everything in the Producer Edition, plus Fruity Video Player, the DirectWave sampler, Harmless, NewTone, Pitcher, Gross Beat, Vintage Chorus, and the Hardcore guitar effects suite. Fruity Video Player allows you to see videos in sync with your music, which is handy for film composers. We will discuss how to use Fruity Video Player to score videos in *Chapter 5, Creating Sheet Music with MuseScore, Scoring with Fruity Video Player, and Diegetic Music.*
- The **All Plugins Bundle** includes all possible plugins and features created by Image Line at the time of purchase, including a large selection of instruments and effects.

There are a lot more features included in each edition. For a full comparison of product versions, visit https://www.image-line.com/fl-studio/compare-editions/.

FL Studio provides lifetime free updates. This helps to ensure that you're always using the latest and greatest features available. Whenever the FL Studio team comes out with new software plugins, you can get them instantly just by updating FL Studio. Over the years, FL Studio has made some major advancements, and the lifetime updates add up over time to be really important.

The official FL Studio YouTube channel, *Image-Line* (https://www.youtube.com/user/imageline), keeps you up to date with new FL Studio features by posting every software update. Whenever FL Studio releases new features, they release a tutorial video, overviewing what's new.

Now that we know what FL Studio is, let's jump in and start using the tools.

## FL Studio core tools

Tools in FL Studio can only be really understood once you start using them. This is a book about composing techniques, so instead of listing every control and feature, we'll just focus on the need-to-know features and then get to the fun stuff – composing. In this chapter, we will cover the basics for beginners. We'll load an instrument into the Channel rack, create a simple melody in the Piano roll, add it to the Playlist, route it to the Mixer, and export it from FL Studio.

At the top of FL Studio, you'll find the menu panel and the player. The menu panel contains the FILE navigation dropdown for creating, saving, opening, and exporting projects. Below FILE, you'll see text describing the name of whatever button your mouse cursor hovers over, as well as a list of keyboard shortcuts for using the control. If you're ever wondering what a plugin or control is called, it will be described here. The following screenshot shows the menu panel and the player:

![](img/e7035ac7133980a2ff428c56c901384d_54_0.png)

Figure 2.2 – FILE

In the preceding screenshot, you'll see a button with PAT and SONG. Left-clicking allows you to toggle between options, switching between playing patterns and songs. Think of a pattern as a group of melodies played by instruments. Songs are bigger than patterns; they consist of multiple music patterns, as well as samples and automation.

To the right of the PAT and SONG buttons are the Play and Stop buttons, which will play your music. Alternatively, you can press the spacebar to start and stop playing audio.

There are five main tool sections in FL Studio, accessible by left-clicking the following buttons:

![](img/e7035ac7133980a2ff428c56c901384d_54_1.png)

Figure 2.3 – Tools

The preceding screenshot lists, from left to right, the Playlist, the Piano roll, the Channel rack, the Mixer, and the Browser. In order to compose successfully with FL Studio, you will need to understand at least the basics of each of these. The following diagram shows how the tools relate to each other:

![](img/e7035ac7133980a2ff428c56c901384d_55_0.png)

Figure 2.4 – FL Studio tools

These tools connect to each other as follows:

- The Browser helps with file navigation. Here, you can grab sound samples and navigate through your folders.
- The Channel rack displays all the instruments, samples, and automation used in your project. This is where you can add new instruments to be used.
- The Piano roll is where you create note melodies for any instrument that you've loaded into the Channel rack.
- The Playlist contains all the patterns, samples, and automation used in your song. This is the dashboard showing all your sounds.
- The Mixer is where you add effects to your sounds. You can route instruments and sounds from the Channel rack to the Mixer and then can edit the sounds in the Mixer.

Don't worry if some of this goes over your head at first; it's a little confusing to understand by short descriptions alone. We will discuss each of these in this chapter.

Logically, the first thing you do when you are creating a song is pick an instrument to create music with. This is done using the Channel rack tool.

## The Channel rack

The **Channel rack** tool holds all of your instruments, samples, and automations, and allows you to navigate between them. Whenever you want to see what instruments you've added to your project, this is the tool that you will be using. Open up the Channel rack by left-clicking on the **Channel rack** icon:

![](img/e7035ac7133980a2ff428c56c901384d_56_0.png)

Figure 2.5 – The Channel rack icon

When you open the Channel rack, you'll see something similar to the following:

![](img/e7035ac7133980a2ff428c56c901384d_56_1.png)

Figure 2.6 – The Channel rack

At the top-left corner of the Channel rack, you'll see the **Channel Options** drop-down arrow. Left-click **Channel Options | Add one**, and you'll see a list of all the instrument plugins available for you. Alternatively, you can right-click on any instrument in the Channel rack to see the same options:

![](img/e7035ac7133980a2ff428c56c901384d_57_0.png)

Figure 2.7 – Inserting an instrument

From the panel, you can left-click on any instrument, and it will be added to the Channel rack. I encourage you to explore the instruments available at your leisure. For now, we'll select the FL Keys instrument:

![](img/e7035ac7133980a2ff428c56c901384d_57_1.png)

Figure 2.8 – FL Keys

The instrument plugin will open. If you press down on keyboard letters or a connected **Musical Instrument Digital Interface (MIDI)** keyboard, you will be able to hear the instrument notes playing.

If dealing with percussion samples, you can create instrument notes directly on the Channel rack by left-clicking on the note buttons in the Channel rack, as shown in the following screenshot:

![](img/e7035ac7133980a2ff428c56c901384d_58_0.png)

Figure 2.9 – Channel rack notes

When you play the pattern, you will be able to hear the instruments playing notes.

On the left-hand side of the Channel rack, you can see the **Mute/Solo** buttons, and the **Channel Panning**, **Channel Volume**, and **Target Mixer Track** controls, as shown in the following screenshot:

![](img/e7035ac7133980a2ff428c56c901384d_58_1.png)

Figure 2.10 – Mute, Pan, Volume, and Mixer Track

The **Mute/Solo** buttons enable you to turn on and off individual instruments. For example, if you hold down *Ctrl* + left-click on the **Mute/Solo** button, it will mute all other instruments. This helps to isolate individual sounds, making it easier to hear one at a time. To turn on all the instruments again, hold down *Ctrl* + left-click on the **Mute/Solo** button a second time.

The **Channel Panning** knob allows you to hear what the audio sounds like coming from the left or right speaker. **Target Mixer Track** shows you which mixer track the instrument is currently routed to. We'll come back to this later in this chapter.

Here are some useful Channel rack shortcuts:

- **Clone channel**: *Alt + C*
- **Delete channel**: *Alt + Del*
- **Re-order channels**: *Alt + up/down*
- **Transpose channel steps**: *Ctrl + Shift + left/right*
- **Zip channels**: *Alt + Z*
- **Filter groups**: *Alt + G*

Whenever you look at the Channel rack, you'll see all the instruments used in your project. If notes are added to one or more instruments in a pattern, you'll see this show up in the Channel rack.

Now that we've learned how to use the Channel rack, let's learn how to add music notes to an instrument using the Piano roll.

## Using the Piano roll

The **Piano roll** is where you compose chord melodies and progressions. This is where all the magic happens for composers. We will come back to the Piano roll again and again throughout this book.
