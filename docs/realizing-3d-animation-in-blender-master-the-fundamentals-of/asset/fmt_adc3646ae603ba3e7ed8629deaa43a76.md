
In fact, the forward movement of these bones along the `Y` axis is the only thing that concerns us for now! Forget about swinging the arms, turning the hips, or even picking up the feet; all that matters for now is moving one foot forward, then the other, then the rest of the body.

**Important note**

“Forward” in this chapter refers to forward from Rain’s point of view, which requires negative numeric changes in our bones’ `Y` `Location` values.

After we’ve finished keying our extremely basic forward movement, we’ll learn how to make everything loop correctly. This requires that we resolve something of a paradox; Rain’s walk cycle must end as it began, but she must also not be where she was...

## Establishing the stride length

Exactly how far will Rain travel in 24 frames, and when will each foot swing forward while the other one stays planted to the ground? We can answer these questions by roughly keying the first `contact` poses. These are the crucial points at which the foot in front will contact the ground to support Rain’s weight, while the foot behind prepares to lift up and swing forward.

**Important note**

I call these “rough” poses because we’re only concerned with the bones’ locations on the `Y` axis for now. Nevertheless, the `Y` locations of each bone must be entered precisely as written in the following instructions using numeric inputs. Even the tiniest discrepancy here, such as transforming one foot by `\-0.99` meters instead of `\-1.0`, can compound over time, causing our independent bones to drift apart as the walk cycle repeats itself.

### Keying the initial stride forward

For our first pose, I’ve prepared some magic numbers for posing Rain’s feet in the `Left Contact` pose. Remember to enter them exactly:

1.  Go to frame `0` (`Left Contact`).
2.  Move `MSTR-Foot_Parent.L` exactly `\-0.2` m in front of Rain on the `Y` axis.
3.  Move `MSTR-Foot_Parent.R` exactly `0.3` m behind Rain on the `Y` axis.
4.  Key the location of `MSTR-Foot_Parent.L`, `MSTR-Foot_Parent.R`, and `MSTR-Pelvis_Parent`.

This is a far cry from the contact pose you saw earlier, but it’ll do for now:

![Figure 8.9: Rain putting one foot forward](image/B17149_08_9.jpg)

Figure 8.9: Rain putting one foot forward

The important thing is that we’ve established the distance between Rain’s feet on the `Y` axis. If one foot is -0.2 meters ahead and the other is 0.3 meters behind, that gives us a total distance of half a meter. Double that and you’ve got the distance that the trailing foot must cover in order to get as far ahead of the opposite foot on the subsequent Contact pose.

This is known as the `stride length` of the walk cycle, a value that is no less critical than the walk cycle period. Together, the stride length and period determine the overall speed of a character’s movement in world space. As it happens, our stride length is 1 meter, for a walking speed of 1 meter every 24 frames. Tidy, yes?

### Two small steps for Rain

Now, to take our first real step forward! We’ll skip ahead and key the Right Contact pose on frame `12`:

1.  Go to frame `12` (`Right Contact`).
2.  Move `MSTR-Foot_Parent.R` exactly `\-1.0` m forward on the `Y` axis.
3.  Key the location of both `IK` foot bones (but not the torso).

There, our first step! Never mind how it looks right now:

![Figure 8.10: Right foot forward 1 meter](image/B17149_08_10.jpg)

Figure 8.10: Right foot forward 1 meter

Now let’s bring the left foot forward to key the final `Left Contact` pose. We will also need to move the torso one meter forward to catch up with everything else:

1.  Go to frame `24` (the other `Left Contact`).
2.  Move `MSTR-Foot_Parent.L` and `MSTR-Pelvis_Parent` exactly `\-1.0` m forward on the `Y` axis.
3.  Key the location of all three bones.

With those last keyframes, we’ve ended the walk cycle on frame `24` exactly as it began on frame `0`, but with one major change – each of the three bones has moved 1 meter forward:

![Figure 8.11: Rain on frame 24](image/Image96487.jpg)

Figure 8.11: Rain on frame 24

Let’s inspect our `Y Location` F-curves in the Graph Editor:

![Figure 8.12: Y Location F-curves of forward-moving bones](image/B17149_08_12.jpg)

Figure 8.12: Y Location F-curves of forward-moving bones

The alternating flat sections for the foot bones represent the time that each foot will spend planted on the ground while the opposite foot swings ahead and the rest of the body moves forward. It’s important that these flat sections stay flat, or the foot will slide while it’s meant to stay in place.

Et voila, two steps – our walk cycle is practically finished!

![Figure 8.13: Three initial key poses](image/B17149_08_13.jpg)

Figure 8.13: Three initial key poses

What’s that? I agree, it looks terr...ific! A terrific walking animation indeed. So terrific, in fact, that we’re ready to make it loop forever.

## Looping the animation

We need Rain to keep walking (okay, shuffling) continuously, but so far, those two steps are all we’ve got. For this to truly be a walk cycle, Rain must keep traveling for the rest of time, just like that bouncing ball we sent away in [*Chapter 2*](#B17149_02.xhtml#_idTextAnchor039).

Let’s start by making the movement of all three bones cyclic:

1.  In the Graph Editor, select all the F-curves for all three bones, `MSTR-Foot_Parent.L`, `MSTR-Foot_Parent.R`, and `MSTR-Pelvis_Parent`.
2.  Use `Set Keyframe Extrapolation` (`Shift` + `E`) | `Make Cyclic`.

**Important note**

Though we are focused on `Y Location` channels for now, remember to make all the other F-curves cyclic as well.

We might be getting somewhere, but Rain is getting nowhere. Here are the resulting `Y Location` F-curves, zoomed out so we can see where this is all going beyond the first second of animation:

![Figure 8.14: Cyclic F-curves](image/B17149_08_14.jpg)

Figure 8.14: Cyclic F-curves

Ugh, how I hate those ugly vertical cliffs! They’re rarely a good sign. This type of periodicity causes Rain’s bones to snap back every 24 frames to their initial position – the result of cyclic F-curves with unequal first and last keyframes. It’s technically repeating, but not the kind of repetition we’re looking for. Rain goes nowhere, whereas we need her to have traveled 1 meter after 24 frames, 2 meters after 48, and so on.

Could `Linear Extrapolation` be the solution? That suffices for the torso, but it fails to repeat the motion of the feet:

![Figure 8.15: Extrapolated F-curves](image/B17149_08_15.jpg)

Figure 8.15: Extrapolated F-curves

What we need is a combination of extrapolation and repetition. We need F-curves that compound, with each repetition, the offset in value between their first and last keyframes; to `repeat with offset`, if you will.

### Offsetting the loop

The property of being cyclical belongs to an F-curve as an `F-curve modifier`. F-curve modifiers work as non-destructive effects, much like the modifiers on a mesh object. The modifiers of the active F-curve can be found in the `Modifiers` tab, located in the right sidebar of the Graph Editor:

![Figure 8.16: Cycles modifier](image/B17149_08_16.jpg)

Figure 8.16: Cycles modifier

It turns out that the `Make Cyclic` operator simply adds a `Cycles` modifier to the selected F-curve!

**Important note**

“Cycles” in this context just means cyclic. The word has nothing to do with the Cycles renderer.

The `Cycles` modifier panel contains some additional settings we desperately need. You’ll have to change them manually one modifier at a time, so it’s a good thing there are only three F-curves that need this treatment:

1.  Navigate to the `Modifiers` tab in the Graph Editor’s right-hand sidebar (`N`).
2.  For the `Cycles` modifier of each of the three `Y Location` F-curves, change both the `Before Mode` and `After Mode` to `Repeat with Offset`.

Here are our Y Location F-curves now:
