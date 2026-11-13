
## Adding the Second Keyframe

We've already got our first **Z Location** keyframe; let's add the second one now:

1. Select the `Z Location` channel.
2. Go to frame **20**.
3. With your mouse cursor in the Graph Editor, press the `I` key. Note that this brings up a different menu than the one we used in the previous chapter:

![Figure 2.15: The menu to insert keyframes in the Graph Editor](image/B17149_02_15.jpg)

Figure 2.15: The menu to insert keyframes in the Graph Editor

1. From the menu, choose `Only Selected Channels`.

By using this menu to insert a keyframe on just the **Z Location** F-curve, we've added just one single keyframe, not a set of three or six as we had been doing before. This has zero effect on our other two F-curves. Why bother inserting extra keyframes where we don't need them?

This second keyframe can be used to determine the high point of the ball's bounce. Right now, of course, it's still on the floor...

## Editing the Value of Keyframes

We're going to use this opportunity to move up the keyframe we've just made so that you can experience editing the value of a keyframe directly in the Graph Editor. We're going to move our second keyframe up by 1 meter, which just happens to be a nice-looking height for a 20-frame bounce.

Let's do it now:

1. Select the `Z Location` keyframe on frame **20**.
2. Press `G` and `Y` and move it up by approximately **1** unit.

As you move that keyframe up on the Graph Editor's `Y` axis, pay attention to what happens in the 3D Viewport:

![Figure 2.16: Moving the keyframe up on frame 20](image/B17149_02_16.jpg)

Figure 2.16: Moving the keyframe up on frame 20

If you're still on frame **20**, you can see the ball move upward! By editing the value of the **Z Location** keyframe, we're affecting the ball's location on the `Z` axis from within the Graph Editor!

> **Tip**
>
> Changes made in the Graph Editor are immediately reflected in other areas according to how they affect the F-curve at the current frame. You don't have to go to the same frame as a keyframe to edit it, but it's often a good idea.

We're halfway to completing our bounce, now that we have a **Z Location** F-curve that shows a definite change. The ball starts on the floor, and then it goes up – next, we just need to get the ball back down.

## Duplicating Keyframes in the Graph Editor

Anything that goes up will have to come down. Making that happen to the ball will be the job of our third **Z Location** keyframe in this sequence, which we are presently to add.

What should the value (i.e., the `Y` axis position) of this keyframe be? The answer is the same value as the first keyframe, whereupon the ball is touching the floor. Our imaginary floor is flat and hasn't gone anywhere since the ball left it after frame **10**. The value for that keyframe happens to be **0.35**. That was easy. Figuring out the time (i.e., the `X` axis position) of this keyframe isn't hard either. If it took 10 frames for the ball to bounce up 1 meter, it should take another 10 frames to fall back down to its original height. 10 frames after frame 20 would put our keyframe on frame **30**.

We know exactly where our third keyframe needs to go, and there are plenty of ways to put it there in Blender. We could, for instance, simply insert another keyframe on frame **10** and move it down. Nothing wrong with that, but there's another way that's even faster – we can simply duplicate the keyframe on frame **10** and move it over to frame **30**. It's already at exactly the right height!

You already know how to move and duplicate keyframes in the Timeline. The method for doing so in the Graph Editor is the same, but with one additional step:

1. Select the `Z Location` keyframe on frame **10**.
2. Press `Shift + D` to duplicate the selected keyframe.
3. Press `X`.
4. Move the duplicate keyframe to frame **30**.

![Figure 2.17: Three keyframes forming a “hump” in the Z Location F-curve](image/B17149_02_17.jpg)

Figure 2.17: Three keyframes forming a “hump” in the Z Location F-curve

Did you catch that extra step I mentioned? We had to press `X`. This was for the same reason we had to press `Y` after grabbing a keyframe to move it up. Keyframes in the Graph Editor can be moved along two axes instead of just one – axes which map to completely different things. When we move keyframes left and right without locking them on the `X` axis, they can move up or down just a little, changing their value. Even a small difference in the value of this third keyframe could give us a strange result later on.

> **Important note**
>
> For the example in the previous paragraph, we will often want to lock our transformations to either the `X` or `Y` axis when moving keyframes in the Graph Editor. Just remember to press `X` or `Y`!

The first three keyframes on the **Z Location** channel now make the ball go up and come back down. Take a look at the shape of the F-curve, though – does this "hump" count as a bounce? Play the animation and see for yourself.

> **Tip**
>
> If you view your animation in the 3D Viewport from the front, you can watch how the ball's path perfectly mimics the shape of the **Z Location** F-curve as it moves from left to right.

If you think that's good enough, you must live underwater. Here on dry land, a ball does not bounce in a sine-wave motion. It does not steadily accelerate upward after leaving the floor, nor does it slow down right before touching it again. To complete this bounce, we're going to need to use what we learned earlier about interpolation and easing.

## Proper Interpolation and Easing for a Bouncing Ball

How can we improve the motion of this "bounce"? The issue can't be the position of our three **Z Location** keyframes in the Graph Editor; we know that the ball has to start on the floor, we know it needs to reach a high point, and then we know it should come back down in the same amount of time. The frames and values of these keyframes are fine. What's wrong is the shape of the F-curve between those keyframes. We simply can't ignore interpolation!

So, what kind of interpolation do we need here? Can you pick it out from the `Set Keyframe Interpolation` menu? Don't say `Bounce` – that's cheating, and besides, you'll see it gives us little control over things such as the height, rate, and number of bounces.

Remember, we're trying to simulate gravity here! Gravity is observed as a constant downward acceleration, acceleration is the rate of change of velocity, and velocity is the rate of change of displacement (i.e., location). The kind of line we need to create, then, has *a rate of change which itself has a constant rate of change*.

I'll spare you the rest of this physics lesson. What this essentially means is that we need a slope with a quadratic function. Let's find that interpolation mode now:

1. Select the first two **Z Location** keyframes on frames **10** and **20**.
2. `Set Keyframe Interpolation` to `Quadratic`.

This gets us halfway there, but the first half of the bounce is still wrong:

![Figure 2.18: A less-than-ideal, shark-fin-shaped F-curve segment](image/B17149_02_18.jpg)

Figure 2.18: A less-than-ideal, shark-fin-shaped F-curve segment

It *eases in*, and we need the segment to do the opposite of that. Good thing we know how to change easing types:

1. Select the first **Z Location** keyframe on frame **10**.
2. `Set Keyframe Easing Type` to `Ease Out`.

Now we have a bounce!

![Figure 2.19: A proper bounce in the Z Location F-curve](image/B17149_02_19.jpg)

Figure 2.19: A proper bounce in the Z Location F-curve

There's just one small matter left – the ball only bounces once.

# F-curve Extrapolation
