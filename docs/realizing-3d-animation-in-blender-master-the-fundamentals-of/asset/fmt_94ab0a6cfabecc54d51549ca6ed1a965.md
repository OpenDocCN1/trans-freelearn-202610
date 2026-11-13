
3. Go to the opposing passing frame (frame `12`) and paste the flipped pose.

4. Insert rotation keyframes for all four bones again.

Rain’s walk cycle is looking better and better. It’s even beginning to look – dare I say it – natural!

## Phase shifting

Let’s try another technique for making a sine wave, this time on the `Y Euler Rotation` F-curve of `MSTR-Hips`:

1. Find the `Y Euler Rotation` F-curve of the `MSTR-Hips` bone.

2. Select the keyframes at frames `0` and `24` and move them up by `5`.

3. Select the keyframe at frame `12` and move it down by `-5`.

    This makes Rain’s hips swing on more than one axis. Not bad so far, but the peaks of this wave should really be at the Passing frames so that the hips help lift up each passing foot as it swings forward.

4. Select all keyframes of the `Y Euler Rotation` F-curve.

5. Move the selection to the right by `6` frames.

The resulting F-curve wave is not unlike the earlier one we made for the `X Location` of `MSTR-Pelvis_Parent`, but this time it only needed three keyframes:

![Figure 8.29: Rotation F-curves of MSTR-Hips](image/B17149_08_29.jpg)

Figure 8.29: Rotation F-curves of MSTR-Hips

Moving all the keyframes of a cyclic F-curve one quarter of a period forward is what we’d call a *90° phase shift* if we wanted to get really technical. We don’t, though, so I’ll just say that keyframes in our walk cycle can be moved outside the initial frame range if we like.

> **Tip**
>
> Want to experiment with the walk cycle? While the animation is playing, you can move the keyframes of an F-curve along the *X* axis in the Graph Editor to alter the phase, or scale the keyframe values to enhance or reduce their intensity. This kind of experimentation can help you improve the result or come up with new ideas.

Look at how Rain walks now! Some very simple keying work was all it took to really sell the appearance of walking. We also saved ourselves some clicking by ensuring that all F-curves we add in the future will automatically be made cyclic as well.

If we were animating Rain for a waist-up shot, we could skip animating the feet and we’d nearly be done. This is often how 3D animators save time:

![Figure 8.30: Example camera view above Rain’s legs](image/B17149_08_30.jpg)

Figure 8.30: Example camera view above Rain’s legs

Our objective is to make a complete walk cycle though, so let’s turn our attention downward to Rain’s feet.

# Advanced footwork

We knew there was more to animating the feet than making them scoot forward – now comes the part where we make them really walk! A lot of animators run into trouble in this part, but by breaking the process down into logical steps as we’ve been doing, we can make things much less complicated.

We’ll begin by animating the left foot over the whole period, from frame `0` to `24`. Then, everything the left foot does, the right foot can do `12` frames later (or earlier). Since we’ve already keyed the `Y Location` F-curves of both feet, the trickiest part is taken care of.

> **Important note**
>
> Nearly all of the work in this section can (and should) be performed in orthographic side-view in the 3D Viewport. Numerical precision is no longer needed, but pay close attention to how posing the foot affects the knee.

## Rolling the foot

For the first half of our walk cycle, the left foot stays in contact with the ground, but that doesn’t mean it holds perfectly still. Here are the poses we’re looking for:

![Figure 8.31: The positions of the left foot at frames 0, 6, and 12](image/B17149_08_31.jpg)

Figure 8.31: The positions of the left foot at frames 0, 6, and 12

Initially, the left foot makes contact in front of Rain at the heel. The rest of the foot then quickly smacks the ground. After a few frames of lying flat, it will then begin to pivot forwards, but at a different point, this time flexing at the ball of the foot and bringing the whole ankle upwards.

These movements of the foot are known colloquially as heel roll and ball roll, or as foot roll altogether. You might suspect that this would be difficult to animate, and if Rain wasn’t rigged with a roll bone on each foot, you’d be right!

Rain’s roll bones are named `ROLL-Foot_Control.L` and `ROLL-Foot_Control.R`. These are the rotating-arrow-shaped bones that live behind Rain’s ankles on the `IK` layer:

![Figure 8.32: Roll bone of the left foot](image/B17149_08_32.jpg)

Figure 8.32: Roll bone of the left foot

Rotating one of these bones on its *X* axis activates a marvelous mechanism of invisible rigging in the foot, allowing the complex action of foot rolling to be achieved with a single control bone.

### Keying the roll bone

Let’s animate the roll bone for the left foot now:

1. Select `ROLL-Foot_Control.L`.

2. Key an `X Rotation` of about `-40` degrees at frame `0` (*Left Contact*).

3. Key an `X Rotation` of `0` at frames `3` (*Left Low*) and `6` (*Left Passing*).

4. Key an `X Rotation` of about `40` at frame `12` (*Right Contact*).

Here’s the resulting F-curve. We’ll tweak it a bit later:

![Figure 8.33: X Euler Rotation F-curve of ROLL-Foot_Control.L](image/B17149_08_33.jpg)

Figure 8.33: X Euler Rotation F-curve of ROLL-Foot_Control.L

Isn’t this bone great? We produced all that motion by keying just one property.

This concludes the first half of the left foot’s 24-frame period. On frame `12`, the right foot lands (or it will after we animate it) and the left foot is ready to take off!

## Picking up the feet

For the first 12 frames of the walk cycle period, the left foot stays on the ground, holding up Rain’s weight. For the following 12 frames, the left foot will no longer touch the ground in any meaningful way; instead, it must lift up so it can swing forward without scuffing the floor.

Lifting the foot completely off the ground can be accomplished by editing the `Z Location` F-curve of the foot’s main `IK` bone.

1. Find the `Z Location` F-curve of `MSTR-Foot_Parent.L`.

2. On frames `15` and `21`, add `Z Location` keyframes that lift up Rain’s left foot.

3. Add a dip to the curve at frame `18` (*Right Passing*), lower than the neighboring keyframes, but still higher than `0.0`, so the foot swings close to the ground as it passes.

4. Like the bouncing ball, the foot won’t slow down before it strikes the ground, so make sure there’s little to no easing out at frame `24`.

The resulting F-curve looks a bit like a camel with two humps. You can tweak it a bit if you like:

![Figure 8.34: Z Location of MSTR-Foot_Parent.L](image/B17149_08_34.jpg)

Figure 8.34: Z Location of MSTR-Foot_Parent.L

While the foot is in the air, the roll bone we animated earlier can do the work of rotating the foot steadily back, ready once again to strike the ground on frame `24`.

Thus the animation cycle of a single foot is nearly complete! The movement still looks a bit wonky at the knees, however. Before we commit to copying the F-curves of the left foot to the right foot, we must perfect our work so that the whole leg moves smoothly.

## Correcting the motion of the legs

Up until this point, we’ve been animating Rain’s left foot as though it were its own little character, with little regard as to how it connects to the rest of Rain’s body. It’s almost as if we think the feet and hips are completely separate body parts!

In one sense, this isn’t wrong. The ends of an `IK` chain are intended to be posed and animated as independent points in space. The pelvis goes in one place, the foot goes in another place, and the `IK` constraint does the rest of the work for us. The bones for the thigh and shin (which are not posed or animated directly) are completely at the mercy of these start and end points. Rain’s legs even stretch so that they always reach their `IK` targets, no matter the distance.
