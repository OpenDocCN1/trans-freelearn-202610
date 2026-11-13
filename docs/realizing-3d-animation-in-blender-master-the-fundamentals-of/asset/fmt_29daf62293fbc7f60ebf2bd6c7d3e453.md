
# Following a path

## World Space and Local Space

Imagine you are seated aboard a moving train. You are, in a sense, parented to the train car. In `World Space`, you’re moving just as fast as the train car, turning as it turns, moving from station to station. In `Local Space`, however, you’re sitting perfectly still, facing forward, not going anywhere. If you stand up and move about the train car, you’ll be moving at a walking speed in `Local Space` while still moving at roughly the train’s speed in `World Space`.

The wheels of our toy train are passengers in the same way; their rotations may be evaluated in the absolute domain of `World Space`, or the relative domain of their `Local Space`. By evaluating the rotation of the `Wheels A` `Target` object in `Local Space`, the constraint will only read its relative rotation with respect to its parent object, `Engine`. The rotation is then applied to the local space of the owner `Wheels F` with respect to its own parent, `Caboose`.

> **Important note**
>
> An object will always possess its own transform properties, but if it has a parent or a constraint, these properties may no longer directly relate to the object’s position in `World Space`. They might only contribute to the final result, or may even cease to have any effect at all. For this reason, the values of these properties are often referred to as *nominal* transform values (“in name only”).

This constraint now has all the correct settings and may be copied to the remaining four wheel objects.

## Copying Constraints

Having perfected the settings of the `Copy Rotation` constraint for `Wheels F`, we want `Wheels E`, `Wheels D`, `Wheels C`, and `Wheels B` to each have their own `Copy Rotation` constraint just like it.

Rather than adding and adjusting the same constraint four more times, Blender has an operator to quickly copy a constraint from one object to another:

1. Select `Wheels B`, `Wheels C`, `Wheels D`, and `Wheels E`.
2. Hold `Shift` and select `Wheels F`.
3. Use `Object` | `Constraints` | `Copy Constraints to Selected Objects`.

This copies the constraint owned by the active object to all of the other selected objects. Now all five wheel objects have identical constraints and can be rotated simply by rotating the remaining front pair, `Wheels A`.

> **Tip**
>
> Those dotted lines connecting all the objects to their parents and constraint targets can be distracting and sometimes even misleading. Consider hiding them by disabling `Relationship Lines` in the `Viewport` `Overlays` menu.

Here’s an updated visualization of the relationships we’ve made so far. The thin gray lines represent constraints, which have the same one-way dependency that parent/child relationships do:

![Figure 4.17: The dependency chart with lines for constraints](image/B17149_04_17.jpg)

Figure 4.17: The dependency chart with lines for constraints

We’ve finished parenting the wheels to the train cars and giving them the appropriate constraints. This train is ready to hit the tracks!

## Editing the Animation Path

Our animation will look sloppy if any one of the train cars strays even slightly from the tracks as it moves. To animate the train using the methods we learned in previous chapters, we’ll have to tweak all six location and rotation F-curves simultaneously for each train car to keep its wheels directly on the rails as the track bends.

Let’s prepare ourselves for what will assuredly be the most tedious and frustrating task we’ve done yet and add those first `Location & Rotation` keyframes:

1. Actually, let’s not.
2. Do not insert location or rotation keyframes.
3. That would be ridiculous.

Can you imagine how difficult that would have been? It was already enough trouble animating the unicycle in the last chapter, and that thing only had to turn once. The `Follow Path` constraint is a much better way of precisely animating objects along a path, especially when that path bends and twists along multiple axes in space.

The path we’ll use in this section is nothing at all like the `Motion Path` feature we used in the previous chapter, which was merely a visual guide. Not only can this type of path be edited directly, it also already exists in your scene.

Inspect the object `Track` and you’ll see that it is not a mesh object. It’s a `curve` object, a versatile type of object useful in both modeling and animation. This one particular curve has been set to extrude the shape of `Rail Profile` along its length, creating two parallel rails. It is also the target of modifiers that duplicate and bend the object `Ties`, creating those wooden railroad ties that cross the rails. Elegantly, this same curve object that determines the path of the train tracks can just as well determine the path of objects constrained to it.

It gets even better – the specific type of curve is a Bezier spline, and you already know how to edit Bezier splines. The control points of this curve object are the same as the Bezier keyframes in an F-curve. It’s like editing a three-dimensional F-curve!

![Figure 4.18: Rails in Edit Mode](image/B17149_04_18.jpg)

Figure 4.18: Rails in Edit Mode

It’s more difficult to edit animation paths after objects have been constrained to them, so we’ll take this opportunity to personalize the railroad and make the path of our train more interesting than the simple oval that it is now.

In addition to the standard controls and operators for selection and transform, there are a few others you ought to know about to edit 3D Bezier curves:

* `Segments` | `Subdivide` inserts control points between two or more selected adjacent control points.
* `Tilt` (`Ctrl` + `T`) twists the curve at selected control points, allowing you to put banking turns and corkscrews in the path.
* `Toggle Cyclic` (`Alt` + `C`) removes the segment joining the first and last control points, if you’d like to have a path that does not loop.
* `Set Handle Type` (`V`) sets the handle type, same as with Bezier keyframes!

Let’s give these controls a try:

1. Select the object `Track` and go into `Edit Mode`.
2. Edit the path to your liking along any axis.
3. Use only `Automatic` and `Aligned` Bezier handle types, and ensure that there are no turns too sharp for the train cars. The train will follow the path no matter what, but we want it to look believable!

> **Important note**
>
> You can bend the curve object Rails to just about any shape you like in `Edit Mode`, but avoid transforming the object itself in `Object Mode`. This could make it more difficult to add constraints to the train cars.

I’ve decided to go wild with my own tracks and create a roller coaster of sorts:

![Figure 4.19: An example of an edited Rails object](image/B17149_04_19.jpg)

Figure 4.19: An example of an edited Rails object

Once you’re satisfied with your railroad, you can leave `Edit Mode`. It’s time to put the train on the tracks!

## Adding a Follow Path Constraint

The `Follow Path` constraint is one of a handful of constraints that evaluates more than just the transforms of the target object. The target has to be a curve-type object, and its data (i.e., the spline we just edited) will be evaluated so that the constraint owner is effectively parented to a point along the spline.

Interestingly, we don’t need to manually move any of the train cars to the tracks. The `Follow Path` constraint will do most of the work for us when we add it:

1. Select any one of the train car objects `Engine`, `Car`, or `Caboose`.
2. Go to `Object Constraint Properties` and add a `Follow Path` constraint.
3. In the new constraint’s properties, change `Target` to `Track`, change `Forward Axis` to `-Y`, and enable `Follow Curve`.
4. Copy the constraint to the remaining train car objects.

> **Important note**
>
> The `Animate Path` button is not as helpful as it sounds. Don’t press it.
