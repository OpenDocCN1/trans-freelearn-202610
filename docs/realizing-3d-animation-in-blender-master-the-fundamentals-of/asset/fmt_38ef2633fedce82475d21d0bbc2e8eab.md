
## `Sync to Audio`

`Sync to Audio` ensures that what we see in the `3D Viewport` during playback keeps pace with the audio so that our judgment as to how well our animation synchronizes won’t be impaired:

![Figure 9.5: Playback settings for audio in the Timeline](image/B17149_09_5.jpg)

Figure 9.5: Playback settings for audio in the Timeline

On top of that, we’ve also enabled `Audio Scrubbing`. Now, each time we scrub or change frames, we’ll hear the tiny blip of audio play on that frame.

Try it! Annoying, right? But also, kind of helpful. Using this feature while scrubbing, we can hear exactly when each word begins without needing to look at the audio waveform all the time.

**Tip**

Another way to stay in sync and keep things organized is to add a marker at the beginning of each word or phrase. I like to rename these markers to reflect the lines being spoken.

This concludes the section on audio! Having ensured our sound is in the right place, we’re ready to animate, knowing that our audio will be in sync with our animation. Well, maybe we can’t know that just yet – but if it is out of sync, at least we’ll know whose fault it is.

# Acting without speaking

Using the audio as our guide, we can cast aside all doubt about the timing of our animation. We know exactly what words Rain will say, when she will say those words, and how long it will take to say them.

But does that mean we should begin by animating her mouth? Not necessarily! In fact, it looks kind of weird if we solely animate that part first. All those little phonemes and syllables that the mouth makes are details, and we mustn’t start with details. Besides, a character doesn’t just speak with their mouth. For this exercise, we’ll start by making Rain speak with her whole face.

## Key facial expressions

If you’ve been thinking ahead, you’ve probably already listened to Rain’s lines a few times and mouthed them out yourself at your desk or in a mirror. That’s great. Keep doing that. As animators, we can rehearse what we’re about to have our characters do by letting the voice actor speak through us, through our own physical bodies, before she possesses her final host body as Rain.

After listening to the audio and animating yourself (so to speak) a few times, try it again without moving your lips so much. This forces you to act with the rest of your face. Where will Rain be looking? Will she nod or shake her head? Will she smile or frown? And what will her eyebrows be doing? These decisions will comprise the first pass of our animation, but try not to think about it – feel it by acting it out!

Here are some key facial poses in my first pass:

![Figure 9.6: Facial expressions of Rain talking](image/B17149_09_6.jpg)

Figure 9.6: Facial expressions of Rain talking

Just because we’ve postponed lip-syncing for a later section doesn’t mean we shouldn’t include the mouth in our poses. Note, however, that we should avoid keying anything below the neck, as it may conflict with a later exercise in which we combine this animation with our walk cycle from earlier. Follow the next steps:

1.  Enable Rain’s `Face Primary` armature layer. The `FK` layer should be visible as well, but we only need `FK-Head` and `FK-Neck` – you can hide the other `FK` bones.
2.  Choose a frame and create a facial pose for it. It doesn’t have to be at the beginning but try to get comfortable using lots of available bones to achieve the look you want.
3.  As with our first foray into character animation, select all visible bones and key their `Location, Rotation & Scale`. We can always delete unnecessary animation channels later.
4.  Now keep going! Continue keying the entire head and face at different frames until you have a nice sequence that vaguely follows the order of facial expressions Rain should make.
5.  You may also wish to consolidate the channel groups for individual bones by selecting them in the `Dope Sheet` and using `Channel | Group Channels` (`Ctrl + G`), though I recommend doing this less aggressively than we did back in [Chapter 7](#B17149_07.xhtml#_idTextAnchor155). For instance, we’ll want to distinguish the keyframes for the jaw from the lips this time:

![Figure 9.7: Example Action Editor with custom channel groups](image/B17149_09_7.jpg)

Figure 9.7: Example Action Editor with custom channel groups

When you’re done, you should be able to see a kind of coherence forming between Rain and her voice. She’s nowhere near speaking, but she’s feeling what’s being spoken, at least. Don’t worry if it’s sloppy; at this stage, something is better than nothing.

## Automatic keyframing and keying sets

With so many little bones to animate, inserting keyframes gets tedious. After making your pose, you have to remember to select everything and insert keyframes for it, or the pose will reset, and you’ll lose your work. These extra clicks and keystrokes add up!

### Keying sets

By now, you’re probably tired of having to pick which properties to key each time you bring up the `Insert Keyframe` menu (`I`). That’s where `Keying Sets` come in.

You can choose an `Active Keying Set` type in the `Keying` menu of the `Timeline`:

![Figure 9.8: Available Keying Sets](image/B17149_09_8.jpg)

Figure 9.8: Available Keying Sets

You’ve seen this before – it’s the same list that comes up in the `Insert Keyframe` menu (`I`).

While a keying set is chosen, `Insert Keyframe` (`I`) will use that keying set instead of showing you the `Insert Keyframe` menu. For instance, if you select `Location` from this menu, pressing `I` in the `3D Viewport` will bypass the usual menu and immediately insert `Location` keyframes for the selection. This is not particularly useful if you want to be discriminating about which properties to key, so for that reason, I almost always select `Available`, a special keying set that only keys existing animation channels of selected bones/objects.

### Auto Keying

If you still feel like you’re pressing `I` one keystroke too many to insert a keyframe, take a look at the button that lets you insert keyframes automatically. It looks like a record button in the `Timeline` header:

![Figure 9.9: Auto Keying](image/B17149_09_9.jpg)

Figure 9.9: Auto Keying

`Auto Keying` inserts keyframes at the current frame for selected bones or objects every time they are transformed – no need to press `I` afterward. I’ve been keeping quiet about this feature until now as it can quickly make a mess of things if one isn’t careful!

Let’s enable `Auto Keying` and then choose an appropriate keying set:

1.  Enable `Auto Keying`.
2.  Pull up the menu next to the `Auto Keying` button and enable `Only Active Keying Set`.
3.  Go to `Keying | Active Keying Set` and select `Available`.

With these settings, we no longer have to manually insert keyframes. Additionally, should we choose to delete any superfluous animation channels as we work, the `Available` keying set ensures that they won’t be accidentally created again.

Of course, even when `Auto Keying` is enabled, pressing `I` to manually insert keyframes is still necessary in cases where we need to key bones/objects that haven’t been transformed. Also, keep in mind that the `Available` keying set cannot create new animation channels – you will raise an error if you try to key a bone that isn’t already animated.

One last thing: be careful! One drawback to `Auto Keying` is how easy it is to accidentally overwrite keyframes or clutter up the `Dope Sheet`.

### Using Auto Keying to refine our face poses

Let’s try out these new settings for the next pass over our animation:

1.  Use the `Dope Sheet` and `Pose | In Between` operators to hold poses and create breakdowns in between keyframes you’ve already made. You may be surprised at how far you get with some fast, “ease-out” style transitions between key poses.
2.  Use the `Graph Editor` to finalize the motion of the neck and head.
