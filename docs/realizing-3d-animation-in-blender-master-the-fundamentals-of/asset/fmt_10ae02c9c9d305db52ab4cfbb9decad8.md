
Does having this many channels and keyframes disturb you? Me too! Let’s use `Collapse Channels` (`Numpad -`) or click the little arrow next to the `Summary` row so we don’t have to think about them:

![Figure 7.19: Collapsing the animation channels](image/B17149_07_19.jpg)

Figure 7.19: Collapsing the animation channels

Now there are only three keyframes, one for each pose! Not such a complex animation after all.

## Editing keyframes in the Dope Sheet

Just because we can’t see all that other data doesn’t mean it isn’t there; we just don’t have to think about it until we’re ready. For now, we can edit the timing of our three poses in the Dope Sheet as though they were three solitary keyframes.

**Important note**

The following operations need to affect all our keyframes. The Dope Sheet cannot edit the keyframes of hidden channels, so make sure that you disabled `Only Show Selected` and enabled `Show Hidden` before following the next steps.

On the other hand, the channels within a collapsed channel-group do not count as being hidden. Whether a channel is collapsed or expanded does not matter in terms of its functionality.

The Dope Sheet holds no surprises when it comes to controls and keyboard shortcuts. Let’s begin by using `Duplicate` (`Shift + D`) to make Rain hold her first two poses:

1. In the `Summary` row of the Dope Sheet, select the keyframe on frame 1.
2. Duplicate the selection, moving the new keyframe to frame 40.
3. Do the same for the keyframe on frame 50, moving the new one to frame 70.

We just refined the timing of our animation a little so that Rain holds each pose for a short while before transitioning to the next one. Duplicating keyframes is a common way of doing this, which is just like how we animated the cube in [*Chapter 1*](#B17149_01.xhtml#_idTextAnchor015).

The shading between keyframes in the Dope Sheet gives us a visual clue as to where the action is happening. The long gray bars between identical keyframes indicate no change, whereas the non-grayed-out intervals between keyframes are where the motion is:

![Figure 7.20: “Held” keyframes in the Summary row of the Dope Sheet](image/B17149_07_20.jpg)

Figure 7.20: “Held” keyframes in the Summary row of the Dope Sheet

Much information is still hidden, though. Are these five keyframes, or more than 5,000? How many keyframes do we really have?

## Summary or “grouped” keyframes

If we use `Expand Channels` (`Numpad +`), we can see how the keyframes in the `Summary` row actually work. Take a look at what happens when we select one:

![Figure 7.21: Selection of keyframes](image/B17149_07_21.jpg)

Figure 7.21: Selection of keyframes

Every keyframe that is visible (or would be visible if the channels were expanded) appears in the `Summary` row. The same is true for the next two rows, which are redundant for our purposes.

If two or more keyframes share the same frame, they get collapsed into one keyframe in the summary row which represents them all. I call these “summary keys”, but you can call them “meta-keys”, “grouped keys”, or even “fool’s keyframes”; there’s no agreed-upon term for what they’re called.

Let’s move further down the hierarchy and select a summary key for one bone:

![Figure 7.22: Selection of all the keyframes for one bone](image/B17149_07_22.jpg)

Figure 7.22: Selection of all the keyframes for one bone

Each bone’s channel group is like its own summary channel, containing summary keys that effectively encode values for multiple channels. For many bones, we may never need to separately edit the keyframes for individual `Location`, `Rotation`, and `Scale` channels. Instead, we can work on a more abstract level, animating all those properties together for one whole “pose” at a time. With so many bones to worry about, we need all the abstraction we can get! We might even need more than what we already have.

## Organizing the Dope Sheet

There’s only so much we can accomplish by editing the keyframes for a whole character animation in just one row. As we work, we must insert and edit keyframes for particular bones instead of whole poses.

There is just one hurdle: expanding the action channel in the Dope Sheet feels more like an explosion than an expansion. There are simply too many bones in this list, even after having hidden a lot of other bones so that they wouldn’t be keyed. Their order from top to bottom doesn’t make much sense either.

Before we continue further with our animation, let’s organize these channel groups. It’ll make the rest of our work go more smoothly.

### Re-grouping channels

Even when the channel groups are collapsed, there are a conspicuous number of rows in the Dope Sheet, enough to make our work harder than it needs to be. Take a look at these channel group names and consider the bones they correspond to:

![Figure 7.23: Too long of a list](image/B17149_07_23.jpg)

Figure 7.23: Too long of a list

Look at how many channel groups there are for all the little bones in Rain’s fingers and face! Some of these bones don’t need to be animated, but a lot of them do, so we can’t just delete this data. Do we really need to see the keyframes for each individual finger and face bone, though? Is it really that important to have separate channel groups for `FK-Pinky2.L` and `FK-Pinky3.L` so that we can distinguish the timing between the two distal phalanges in Rain’s left pinky finger? Maybe later, but not for this animation.

Instead, we can use the `Channel | Group Channels` (`Ctrl + G`) operator to consolidate the many channels for Rain’s finger and face bones into just a few channel groups.

To start, we should cull some visible bones and channels:

1. Hide the `IK` and `FK` armature layers, leaving only the `Fingers` and `Face Primary` layers visible.
2. Enable `Show Only Selected` in the Dope Sheet.

That should help reduce the danger of selecting the wrong bone or grouping a channel without meaning to.

**Important note**

Be sure of your selection before grouping channels in the Dope Sheet. If you accidentally group the wrong channels, there’s no automatic way (besides `Undo` (`Ctrl + Z`)) to restore how they were grouped initially.

#### Grouping finger bone channels

Now it’s easier to group the channels we want with `Channel | Group Channels` (`Ctrl + G`). Let’s start with the fingers:

1. In the 3D Viewport, select the visible finger bones in Rain’s left hand. This makes them visible in the Dope Sheet.
2. In the channel list of the Dope Sheet, make sure all the channel groups for the selected bones are themselves selected with `Select All` (`A`).
3. Use `Channel | Group Channels` (`Ctrl + G`), and name the new group `Fingers.L`.
4. Repeat steps 1–3 for the finger bones in the right hand and name the channel group `Fingers.R`.

Instead of 38 channel groups (one for each finger bone), we now have just two (one for each hand):

![Figure 7.24: Two channel groups for the fingers](image/B17149_07_24.jpg)

Figure 7.24: Two channel groups for the fingers

The summary keys in these channel groups will work as expected as well. If we keep the channel groups collapsed and continue keying the fingers all together, we can think of each hand as its own character, animated with summary “finger keyframes” that encode each “finger pose”.

That’s easier than dealing with the summary keys for 38 individual bones and much easier than dealing with the 342 individual `Location`, `Rotation`, and `Scale` channels for those bones (which, by the way, are still there):

![Figure 7.25: Aargh! Put it away!](image/B17149_07_25.jpg)

Figure 7.25: Aargh! Put it away!

As you can see, the channel groups can’t be nested. I wish they could. Ask the developers about it. We can’t put channel groups inside other channel groups; we can only empty them of their individual animation channels and put those channels into a new channel group.

**Important note**
