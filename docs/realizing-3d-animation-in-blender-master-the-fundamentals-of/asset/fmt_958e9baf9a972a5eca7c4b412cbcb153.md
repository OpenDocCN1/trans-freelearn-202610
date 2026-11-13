
Like the objects we animated in previous chapters, the bones in an armature all have location, rotation, and scale properties. Transforming one or more bones directly is called **posing**, the first step in character animation. Together, the given positions of the bones in an armature at a single point in time are referred to as a **pose**. As we animate Rain in later chapters, we’ll put her in one pose, key the properties of those bones we just posed, then move on to the next pose at another frame, key the properties for those bones, and so on.

That’s character animation. It’s not all that different from animating objects! This time though, our objects are bones, there are hundreds of them, and they’re connected in a complex system that may not seem intuitive at first. Easy, right? Maybe not. Before animating Rain, we’d better spend a section or two just on how to pose her.

Our first exercise will take Rain from a *T* pose to an idle, natural-looking standing position:

![Figure 6.19: Resting “T pose” to idle standing pose](image/Figure_06.19.0_B17149.jpg)

Figure 6.19: Resting “T pose” to idle standing pose

If we want it to look natural, even a simple standing pose requires manipulating a lot of bones.

## Pose Mode

To start posing Rain, select her armature and go to `Pose Mode` (`Ctrl` + `Tab`) within the 3D Viewport:

![Figure 6.20: Pose Mode in the Interaction Mode menu](image/Figure_06.20.0_B17149.jpg)

Figure 6.20: Pose Mode in the Interaction Mode menu

In `Pose Mode`, bones may be selected and transformed much like regular objects in `Object Mode`, using the same standard controls for `Move` (`G`), `Rotate` (`R`), and `Scale` (`S`).

Posing an armature in `Pose Mode` is technically distinct from editing an armature in `Edit Mode`, which is what the rigger does. We can’t edit the bones in a linked armature object and we shouldn’t ever need to while animating.

**Important note**

While in `Pose Mode`, you cannot add or select other objects; you must return to `Object Mode` to do so.

Because Rain’s control bones have complex relationships and do not directly deform her mesh, the result of transforming a given bone is not always predictable. Though the shape of a given bone and its location on Rain’s body offers clues as to that bone’s function, the only sure way to know what a bone does is to see what happens when we move it.

**Tip**

Don’t be afraid to move a given bone to see what it does. You can always `Undo` (`Ctrl` + `Z`), or use right-click or `Esc` to cancel the transformation before you confirm it. Additionally, in the next section, I’ll show you how any change you make to a bone in `Pose Mode` can be reset.

### Posing the Torso and Hips

Which bone do we start with? When initially posing a character, it’s often best to start with the bone that controls the overall position of the torso. In Rain’s case, this is `MSTR-Pelvis_Parent`. If you’re having trouble finding it, it’s the four-leaf-clover-shaped bone around Rain’s hips.

**Tip**

To view the names of Rain’s bones at a glance, go to the armature’s `Object Data Properties` area, find the `Viewport Display` panel, and enable `Show` | `Names`. You’ll probably want to disable this later when we un-hide more armature layers, though.

We’re going to use `MSTR-Pelvis_Parent` to give Rain’s legs a little room to bend:

1.  Enter `Pose Mode` and select `MSTR-Pelvis_Parent`.
2.  Move `MSTR-Pelvis_Parent` down about **-0.02** meters on the global *Z* axis.

Now let’s swing Rain’s hips to her right using the saddle-shaped bone known as `MSTR-Hips`:

1.  Select `MSTR-Hips`.
2.  Rotate `MSTR-Hips` about **10** degrees on the global *Y* axis.

Even with these subtle changes to just two bones, Rain appears more relaxed and natural. She’s beginning to look less like a mannequin and more like a real person!

![Figure 6.21: Subtle difference in pose](image/Figure_06.21.0_B17149.jpg)

Figure 6.21: Subtle difference in pose

Note that both `MSTR-Pelvis` and `MSTR-Hips` move Rain’s hips, but one is the child of the other and has a more localized effect. This extreme example shows the difference:

![Figure 6.22: Making Rain squat with MSTR-Pelivs versus MSTR-Hips](image/Figure_06.22.0_B17149.jpg)

Figure 6.22: Making Rain squat with MSTR-Pelivs versus MSTR-Hips

Just because a bone can be moved doesn’t always mean it ought to be! That said, you should feel free to experiment. In pose mode, all changes to bones are reversible, and there’s no danger of accidentally deleting or breaking anything. If any bone gets bent too far out of shape, just remember the following operators to reset it...

### “Un-posing” a Posed Bone

Every bone has a **rest position** – the initial position it starts with before we transform it in `Pose Mode`. After posing bones, we’ll often want to reset them to their rest positions later. This is useful for “relaxing” a bone out of one pose before putting it in the next one, to make sure that Rain doesn’t walk around with any slightly dislocated joints, or just to return to a state of sanity after getting silly...

![Figure 6.23: There’s an easy way to fix this](image/Figure_06.23.0_B17149.jpg)

Figure 6.23: There’s an easy way to fix this

In the preceding example, I’ve put `MSTR-Pelvis` in a ridiculous place. To fix this, there’s no need to try and manually put `MSTR-Pelvis` back where it was, or to reset all its transform values numerically. All I need to do is select the offending bone and go to `Pose` | `Clear Transform`:

![Figure 6.24: Clear Transform operators](image/Figure_06.24.0_B17149.jpg)

Figure 6.24: Clear Transform operators

Using `Pose` | `Clear Transform` | `All` puts `MSTR-Pelvis` back to where it started, a position from which it is easier to start posing again.

**Tip**

In addition to `Pose` | `Clear Transform` | `All`, the three main `Pose` | `Transform` operators (`Move` (`G`), `Rotate` (`R`), and `Scale` (`S`)) each have an associated `Pose` | `Clear` Transform operator: `Location` (`Alt` + `G`), `Rotation` (`Alt` + `R`), and `Scale` (`Alt` + `S`), respectively. These work the same as the `Object` | `Clear` operators in `Object Mode`, but they’re especially useful in character animation.

If we know how to quickly reset any bone, we’ve nothing to fear when it comes to experimenting while posing. Any change we make in `Pose Mode`, no matter how absurd or distorted, can be reversed.

## IK Bones

Look at Rain’s feet and notice how they remained stationary while we moved her hips and torso, causing her knees to bend. Currently, Rain’s feet are controlled by the `MSTR-Foot_Parent.L` and `MSTR-Foot_Parent.R` bones, which move independently from the bones we just moved earlier by default.

**Tip**

Because people and creatures tend to be symmetrical on the *X* axis, a lot of armature bones come in mirrored pairs – one for the left-hand side and one for the right-hand side. The bones in these pairs will have identical names except for the suffix of *L* or *R*.

Let’s see what happens when we move the bone for the left foot:

1.  Select `MSTR-Foot_Parent.L` (this is the shoe-shaped bone around Rain’s left shoe).
2.  Rotate the bone out about **20** degrees on the global *Z* axis.
3.  Move the bone to Rain’s left, about **0.2** meters on the global *X* axis.
4.  Move the bone back about **0.1** meters on the global *Y* axis.

A few more asymmetries contribute to the relaxed look of Rain’s pose:

![Figure 6.25: A casual, natural-looking leg stance](image/Figure_06.25.0_B17149.jpg)

Figure 6.25: A casual, natural-looking leg stance
