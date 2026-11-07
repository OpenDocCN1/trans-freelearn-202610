# Coordinates in Godot

An important quirk of screens is that the origin (0, 0) is in the top-left corner of the screen. This means that y is negative as we move upward and positive as we go downward. This is shown by the vector (4, 3) in Figure 5.6:

![](bbox=[0.33, 0.22, 0.65, 0.5])

Figure 5.6 – Computers render the screen downward from the top left (This image is by Godot Engine contributors and is licensed under CC BY 3.0)

In Figure 5.6, the vector (4, 3) has a direction or rotation of angle theta and a length or magnitude of m. In this case, the arrow is a position vector indicating a point in space relative to the origin.

One important thing to remember about vectors is that they usually show direction and size (how far or fast something moves), but not the starting position. Think of a vector like an arrow: it shows you which way to go and how fast, but not where you are on the map. To know the actual position of an object, you need to combine the vector with a starting point or reference.

However, there’s a special case that’s worth mentioning: position vectors. In mathematics and game development (including Godot), a vector such as Vector2(2, 3) can also represent a position in 2D space. This type of vector is understood to start at the origin (0, 0) and point to the coordinates (2, 3). In this case, the vector tells you where something is located relative to the origin.

So, depending on the context, vectors can do either of the following:

+   - Show how far and in what direction something is moving (velocity or force)
+   - Indicate a position relative to the origin (like coordinates in space)