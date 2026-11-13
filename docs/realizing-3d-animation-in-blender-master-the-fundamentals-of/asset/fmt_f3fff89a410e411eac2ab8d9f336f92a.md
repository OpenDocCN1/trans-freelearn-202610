
All three train cars should have a constraint that looks like this:

![Figure 4.20: The Follow Path constraint settings](image/B17149_04_20.jpg)

Figure 4.20: The Follow Path constraint settings

The cars themselves, meanwhile, are *almost* all on the rails.

### Correcting path offsets

If your train cars happened to be placed onto a curved portion of the tracks after you added their constraints, only one of them, `Car`, will actually be on the tracks. The other two cars just seem to stick out from the middle one, as if the whole thing is one straight train car:

![Figure 4.21: Train cars displaced from rails](image/B17149_04_21.jpg)

Figure 4.21: Train cars displaced from rails

In its current predicament, this train will not bend properly around the track. Why is this happening, and how can we fix it?

Consider what happened when you added the first `Follow Path` constraint. The train car seemed to automatically snap to a point somewhere along the spline of `Track`. Presently, this invisible point is at the starting position of the path (somewhat arbitrary if the curve is cyclic). This point’s position along the curve can be animated (and will be, later), and the `Follow Path` constraint owner will effectively be parented to it. The offset of the constrained object from this point along the curve is equal to the transform difference between the constraint owner and the target curve object, evaluated before the effect of the constraint.

Now consider where the train cars were before you constrained them. `Engine`, `Car`, and `Caboose` have the nominal locations `[0, -3, 0]`, `[0, 0, 0]`, and `[0, 3, 0]`, respectively. The origin of the curve object `Track` is located at `[0, 0, 0]`. Therefore, only the origin of `Car` will be constrained correctly along the curve, whereas the other two train cars “stick out” on the local `Y` axis by 3 units, one forward and one backward.

Our solution is to “reset” the location values of those other two train cars and instead adjust their relative positions along the curve, using the offset value of their `Follow Path` constraints:

1. Select the object `Engine`.
2. Use `Object` | `Clear` | `Location` (`Alt` + `G`) to reset the object’s nominal location to `[0, 0, 0]`.
3. Adjust the `Offset` value of the `Follow Path` constraint so that `Engine` is again coupled with `Car` at the appropriate distance. The exact value will vary depending on the total length of the path that you edited.
4. Repeat *steps 2* and *3* for `Caboose`. The correct `Offset` value will most likely be the exact inverse of the one for `Engine`.

Now every train car’s origin lies precisely along the curve:

![Figure 4.22: Correctly offset train cars](image/B17149_04_22.jpg)

Figure 4.22: Correctly offset train cars

The train cars’ position along the track can now be animated with a single property. That property, however, might not be where you expect...

## Animating the path

You might have guessed that we would animate the object `Engine` and constrain the other two train cars behind it, similar to how a train works in real life. Nothing of the kind. Instead, the three train cars now each share a single constraint target, `Track`. We will animate the whole train by animating a single property of the target’s data.

**Important note**

The term `data` in that last sentence is vital. An object and its data are two distinct datablocks with their own properties, which are found in separate tabs of the Properties Editor. The object `Track` is a user of curve data that is also named `Track`. These names are arbitrary and do not have to match.

After selecting the curve object `Track`, the property we’re looking for can be found in the `Path Animation` panel of the `Object Data Properties` tab of the Properties Editor:

![Figure 4.23: The Path Animation panel](image/B17149_04_23.jpg)

Figure 4.23: The Path Animation panel

These properties work in conjunction with any `Follow Path` constraints that use the curve as a target. The `Frames` property defines how many “frames” should pass for constrained objects to traverse the total length of the curve. “Frames” is in quotes here because, “frames” in this context refers to arbitrary units of the `Evaluation Time` property, which can be animated at any rate we like.

In truth, these properties have nothing necessarily to do with frames or time. `Evaluation Time` is a numerator, `Frames` is a denominator, and the divided result equals the relative position of constrained objects along the total length of the path. If we leave the value of `Frames` at `100`, we can think of `Evaluation Time` as a percentage!

### Keying Evaluation Time

The functionality of these `Path Animation` properties is contrived and they’re badly labeled. Even the tooltip text appears to be lying. Such is the delightful quirkiness of open-source projects.

Let’s have ourselves a hearty chuckle and insert our two initial keyframes:

1. Select the object `Track` and find the `Path Animation` panel in the `Object Data Properties` tab of the Properties Editor.
2. Go to frame `0` (yes, frame `0`) and insert a keyframe for `Evaluation Time`.
3. Go to frame `250` (the end frame), change `Evaluation Time` to `100`, and insert another keyframe.

You can now watch the train move along the total length of the track!

### Refining the speed of the train

Depending on the length of your track, you may find that the default animation duration of 250 frames is not enough time for the train to cover the length of your track without moving too fast, or perhaps 250 frames is too long and the train moves too slowly.

If you’re happy with the speed of your train and the duration of your animation, you can skip this step. Otherwise, let’s adjust some things so that we get the general speed that we want:

1. Using either `Linear` interpolation or `Vector` Bezier handles, make the `Evaluation Time` F-curve linear. This lets us watch the train go by at its average speed.
2. Grab (`G`) the second `Evaluation Time` keyframe and move it to a new frame, giving the train a different speed.
3. Adjust the value of `End Frame` so that the animation ends on the same frame as the second keyframe you just moved.
4. Repeat *steps 2* and *3* until you are happy with the average speed of the train.

Once we’ve determined the optimal duration of our animation and the train is moving at a decent average speed, it will be easier to insert more keyframes to vary its speed based on the shape of the path that it follows.

### Points of inflection along peaks

If your track goes up and down at all, you may wish to vary the `Evaluation Time` F-curve to accordingly vary the speed of the train. We want the train to speed up as it goes downhill and slow down as it goes uphill. This means that each change in direction between uphill and downhill (i.e., a peak or trough) should be accompanied by a point of inflection in the F-curve.

Using keyframes with `Aligned` Bezier handles, we can create our own points of inflection, making the F-curve flatter at the peaks of the path:

![Figure 4.24: A keyframe at a high point in the example path](image/B17149_04_24.jpg)

Figure 4.24: A keyframe at a high point in the example path

And we can make it faster at the troughs:

![Figure 4.25: A keyframe at a low point in the example path](image/B17149_04_25.jpg)

Figure 4.25: A keyframe at a low point in the example path

Now it’s your turn:

1. Scrub through your animation in the `Graph Editor`.
2. Find each frame at which the train is at a peak or trough in the tracks, and mark it by inserting a keyframe on the `Evaluation Time` F-curve.
3. Convert all keyframes to `Aligned` Bezier keyframes.
4. Rotate (`R`) the keyframes according to the shape of the path, making them flatter at peaks and more vertical at troughs.
