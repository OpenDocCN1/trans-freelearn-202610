
# The origin and the object

So you want to “move the origin,” do you? But not the object? If the location of the origin can differ from the location of the object, then what is the location of the object, exactly? Let’s look at the location of one of the wheels:

![Figure 4.4: The location of one of the wheel objects](image/B17149_04_4.jpg)

Figure 4.4: The location of one of the wheel objects

That can’t be right, can it? `[0.0, 0.0, 0.0]` is the global center of the scene, where the origin is. So where’s the object? What even is the difference between an object and an origin? We’ve been animating objects this whole time, but are you even sure what an object is?

Ready for the truth? Look at that orange dot again. *That’s* *the object.*

![Figure 4.5: The object](image/B17149_04_5.jpg)

Figure 4.5: The object

The origin and the object are essentially the same thing. An object in Blender is merely a point in space, with a name and basic properties such as location, rotation, and scale. These points are the only things that get considered in most object relationships. We’ve been moving origins since [*Chapter 1*](#B17149_01.xhtml#_idTextAnchor015)!

Whenever we refer to something called the origin, we’re talking specifically about that point and nothing else. Everything else attached to that point is the object’s data. In the case of mesh-type objects like our wheels and train cars, this means the mesh geometry (that is, all the vertices, edges, and faces exposed to you in `Edit Mode`).

**Tip**

An `empty` object is a special type of object in Blender that has no data. Like a mesh object with all of its vertices deleted, it is essentially nothing but an origin. “Empties” can be used in relationships just like any other object but are never rendered. You can add one by going to `Add` | `Empty`.

So, you can’t move the origin separately from the object because the terms *object* and *origin* have no meaningful distinction. The task is null. Meaningless. Less than impossible.

Moving the object *inversely to its geometry*, however, is no problem at all.

## Centering the origin

Blender offers a couple of operations and settings that refer to the origin as something distinct from the object, which can be moved “by itself.” This is convenient for us, but it masks a more complex operation in which two things move simultaneously – the object in one direction and the mesh geometry in the other direction.

Let’s get back to those wheels. There are 12 wheels but only 6 wheel objects. Each one is a pair of wheels with left/right symmetry. If each of these six wheel-pairs is to rotate properly about six imaginary axles, we must, in a manner of speaking, “move the origins” to where the centers of rotation ought to be.

This can be accomplished with a single operator:

1. Select all six wheel objects.
2. Click `Object` | `Set Origin` | `Origin to Geometry`.

![Figure 4.6: Six centered origins for the wheel objects](image/B17149_04_6.jpg)

Figure 4.6: Six centered origins for the wheel objects

Now each wheel object is in the right location, perfectly centered with its geometry. All the wheels can now rotate correctly on the `X` axis.

**Tip**

When calculating the center of a mesh, the operators `Origin to Center of Mass (Surface)` and `Origin to Center of Mass (Volume)` can sometimes yield more favorable results on complex models. In our case, it makes no difference for the wheels, which have simple geometry.

It looks like we “moved the origins” to the center of each pair of wheels, but we really didn’t! Instead, for each pair of wheels, the global center of the mesh geometry was calculated, the object itself was moved to that point, and its mesh geometry was moved in the inverse direction. It’s as if we moved the object in `Object Mode` and then went into `Edit Mode`, selected everything, and moved it back.

The effect makes it look as though only the origin has moved. However, we know better!

**Tip**

In addition to the technique in the following section, you can also relocate an origin anywhere by moving the 3D cursor to the desired spot and then using `Object` | `Set Origin` | `Origin to 3D Cursor`.

## Grabbing the origin

The labeling of certain operations and settings in Blender’s interface suggests that Blender really wants us to believe that objects possess an origin that can be moved by itself. Although we know better, we’ll continue to indulge the developers in their fantasy and let them think they have us fooled. Sure, let’s “move” some more “origins.”

Let’s turn our attention to the three main components of the train, which we will later constrain to the tracks – the `Engine` object at the front, `Car` in the middle, and `Caboose` at the back. Unlike the wheels, their origins do not have to be exactly at the centers of their geometry; in fact, it will make things easier for us if we keep them on the ground and space them exactly **3** meters apart. Using one of the `Set Origin` operators to center the origins would only complicate things, as each train car has different geometry.

The `Car` object is already exactly where it needs to be. We just need to move `Engine` and `Caboose` so that the origins are below the respective meshes in the same way. Doing so requires us to enable just one setting before grabbing the objects as we normally would:

1. In the header of the 3D Viewport, go to `Options` | `Transform` | `Affect Only` and enable `Origins`.

![Figure 4.7: Affect Only | Origins enabled in the 3D Viewport](image/B17149_04_7.jpg)

Figure 4.7: Affect Only | Origins enabled in the 3D Viewport

1. Select the object `Engine`.
2. Use `Grab` (`G`), press `Y`, and move the origin **\-3** units forward on the `Y` axis so that it rests below the train engine mesh.
3. Select the object `Caboose`.
4. Use `Grab` (`G`), press `Y`, and move the origin **3** units back so that it rests below the caboose mesh.
5. Remember to disable `Options` | `Transform` | `Affect Only` | `Origins` when you’re finished.

The three objects are now evenly spaced 3 units apart:

![Figure 4.8: Three train car origins](image/B17149_04_8.jpg)

Figure 4.8: Three train car origins

Remember, we haven’t actually “moved only the origins” in the way that you might be led to believe. What we just did was move objects like we normally do, only this time, as we did so, the meshes of those train cars also got moved in the opposite direction. It’s the same as walking on a treadmill – you’re moving forward relative to the conveyor belt while remaining in the same location globally. When `Affect Only` | `Origins` is enabled, our transform operations affect both the object and the mesh data at once. We didn’t affect “only” one thing at all; we affected more than what we would have otherwise!

We’ve corrected all the object origins – or, more accurately, we’ve corrected the locations of our objects and the relative offset of their respective meshes. Next, we will explore a way to – in a sense – make one object the origin of another.

# Parenting objects

Whereas the term *origin* refers to an object’s position in relation to its own data, in this section, we will learn the most basic relationship between two different objects – that of `parent` and `child`.

In Blender, parenting is when you “attach” one or more objects to another object, allowing the parent object and all its children to be moved and animated as one.

Objects can be parented to other objects using the `Set Parent To` menu (`Ctrl` + `P`):

![Figure 4.9: The Set Parent To menu](image/B17149_04_9.jpg)

Figure 4.9: The Set Parent To menu

**Important note**
