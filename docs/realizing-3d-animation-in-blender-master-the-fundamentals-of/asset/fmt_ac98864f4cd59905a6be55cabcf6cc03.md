
# Torso Animation Basics

## Bouncing Down and Up

Figure 8.23: F-curves of `MSTR-Pelvis_Parent`

Now it's your turn.

After `Y Location`, the next most important animation channel of `MSTR-Pelvis_Parent` is `Z Location`, which conducts the up-down motion of the body that everybody makes when they walk. This is what gives the `High` and `Low` walk cycle phases their names; the torso usually reaches its high point just before the front foot makes contact, and its low point right after.

Try it now:

1. Find the `Z Location` F-curve of `MSTR-Pelvis_Parent`.
2. Insert a keyframe at frame `3` (`Left Low`) so that Rain's torso reaches a low point, about -2 cm (`-0.02` meters).
3. Insert a keyframe at frame `9` (`Left High`) so that Rain's torso reaches a high point just above zero, about 0.5 cm (`0.005` meters).
4. To make Rain's low point a bit briefer, add a keyframe at frame `6` (`Left Passing`) that raises the torso just a little higher than it would've been otherwise.
5. Shorten the period of the F-curve by moving the last keyframe from frame `24` to frame `12`.

Note that this up-down motion needs to happen twice in a period – once for each footstep. Instead of duplicating our keyframes, we simply made the period of this F-curve 12 frames instead of 24.

## Swaying from Side to Side

In addition to up-down motion along the `Z` axis, a walking torso will also tend to sway side-to-side on the `X` axis, as the weight of the torso is held in alternating imbalance above one foot or the other.

> **Tip**
> The `Active Keyframe` panel, located in the right-hand sidebar of the `Graph Editor`, is handy for inspecting the exact values of keyframes.

Use any method you like to key this motion:

1. Find the `X Location` F-curve of `MSTR-Pelvis_Parent`.
2. Insert keyframes at both Passing frames, making Rain's whole torso sway to the left at `Left Passing` and to the right at `Right Passing`.

The amplitude of this wave should be subtle, about 1 cm (0.01 meters) in either direction. See the previous figure for reference.

Isn't it amazing how some subtle keying along two F-curves can bring our character to life? In fact, that last `X Location` F-curve is literally just a sine wave. Wait 'til you see what some more of these waves will do for Rain's hips and chest!

# The Action Editor

From this point forward, literally every F-curve we add to our animation will need to be cyclic, but F-curves are not cyclic by default. One thing that could get annoying is having to perform `Make Cyclic` with each new F-curve that gets created.

Before we key any more bones, we'll address this issue in an editor that is way less exciting than it sounds: the `Action Editor`.

## Intro to the Action Editor

Remember actions? They're the data blocks that contain multiple animation channels and channel groups for each animated object. Our whole walk cycle for Rain is contained within one of these actions; it was automatically created and given a generic name when we inserted our initial keyframes.

Actions have some basic metadata that can be accessed through the Action Editor, which can be found by going to the Dope Sheet and changing the editing context:

![Figure 8.24: The Action Editor in the Dope Sheet](image/B17149_08_24.jpg)

Figure 8.24: The Action Editor in the Dope Sheet

The `Action Editor` is nearly identical to the standard `Dope Sheet`. Its main difference is that it displays only one action at a time: the current action of the selected object. It also lets us edit some additional properties of the action, such as its name.

## Renaming an Action

First, let's use the `Action Editor` to rename Rain's current action:

1. Bring up the `Dope Sheet`.
2. Change the `Editing Context` of the `Dope Sheet` to `Action Editor`.
3. Rename the action to something you'll remember.

Renaming this action will make it easier to find so we can reuse it as an asset in future animations:

![Figure 8.25: "Rain Walkcycle" is a perfect name](image/B17149_08_25.jpg)

Figure 8.25: "Rain Walkcycle" is a perfect name

> **Tip**
> From here on out, you can keep the `Action Editor` open instead of the default `Dope Sheet`. It makes little difference since we won't be animating any other objects or working with any actions besides the current one for Rain's armature.

In addition to renaming actions, the `Action Editor` does another thing – maybe even two other things! Let's use it to enable some properties that establish the whole action as a cycle.

## Establishing an Action as Cyclic

Some additional action metadata can be found in the right-hand sidebar of the `Action Editor`. Modifying some properties here will make our work easier:

1. Find the `Action` panel in the right-hand sidebar (`N`) of the `Action Editor`.
2. Enable `Manual Frame Range` and ensure that the `Start` and `End` frames of the action are `0` and `24`.
3. Enable `Cyclic Animation`.

Note that these settings do not directly alter the F-curves or affect the animated result:

![Figure 8.26: Cyclic Animation enabled for the action](image/B17149_08_26.jpg)

Figure 8.26: Cyclic Animation enabled for the action

All we did was to change some contextual information about the action so that some other features know it's supposed to be cyclic.

### Enabling Cycle-Aware Keying

For the settings to have any effect, we'll need to enable `Cycle Aware Keying` in the Timeline like so:

1. Open the `Keying` popup in the header of the `Timeline`.
2. Enable `Cycle Aware Keying`.

Now, all future F-curves added to the `Rain Walkcycle` action will automatically be cyclic and have a period of 24 frames:

![Figure 8.27: Cycle-Aware Keying enabled in the Timeline](image/B17149_08_27.jpg)

Figure 8.27: Cycle-Aware Keying enabled in the Timeline

Let's try it out on some more bones.

# More Torso Animation with Sine Waves

The hips, chest, and shoulders are part of a walk cycle too. Some very simple keying work on these bones will go a long way to loosening up Rain's movement and making her walk cycle more believable.

## Twisting the Torso at the Contact Poses

We'll begin with some rotation along the `Z` axis at the `Contact` poses. Because feet are typically farthest apart at the `Contact` phases of a walk cycle, the hips will rotate to help cover this distance (after all, they are directly connected to the legs). Consequently, the chest and shoulders should rotate inversely to counterbalance this motion:

![Figure 8.28: Top view of Contact poses with rotating hips, chest, and shoulders](image/B17149_08_28.jpg)

Figure 8.28: Top view of Contact poses with rotating hips, chest, and shoulders

Give it a try:

1. Go to frame `0` (`Left Contact`).
2. Rotate `MSTR-Hips` about `-10` degrees on the `Z` axis in the direction of Rain's stride (left side goes forward, right goes back).
3. Rotate `MSTR-Chest_Parent` about `5` degrees on the `Z` axis in the opposite direction.
4. Continuing the gesture of the torso through to Rain's shoulders, rotate `MSTR-Clavicle.L` outward and `MSTR-Clavicle.R` inward a few degrees on the `Z` axis.
5. Insert rotation keyframes for these bones.

Notice that, with `Cycle-Aware Keying` enabled, keyframes have also been added at frame `24`. This saves us the trouble of keying the same thing at both `Left Contact` poses. The F-curve has also already been made cyclic for us.

Now we only need to insert keyframes for the `Right Contact` pose and we're done!

### Mirroring the Rotation of the Torso

`Paste Flipped Pose` (`Ctrl` + `Shift` + `V`) doesn't just work for bones ending in `L` or `R`; it's also useful for mirroring the position of bones along the sagittal plane (the ones in the middle of the character that have no L/R suffix, such as the hips and chest).

Try it now to save a little time:

1. Select `MSTR-Hips`, `MSTR-Chest_Parent`, and both `MSTR-Clavicle` bones.
2. Copy the pose.
