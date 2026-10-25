# # Working with matrices

A **matrix** is a rectangular array of elements arranged in rows and columns. It is convention to specify the number of rows first when referring to the size of a matrix, so a 3-by-2 matrix, for example, would be:

![](bbox=[0.4668, 0.2344, 0.5332, 0.282])

Each element in a matrix can be thought of as a component that maps some point to another point. The point (_x_, _y_), for example, can be mapped to (_p_, _q_) as follows:

![](bbox=[0.3291, 0.3458, 0.6708, 0.3771])

This matrix defines a transformation from one point in 2D space to a new point in 2D space. The new _x_-point, _p_, is scaled by 2 while the new _y_-point, _q_, is scaled by 2 and translated by the original _x_ (this effect is known as **shear**).

# # Transformation matrices

Transformations in Raphaël are known as **affine transformations**. The idea is that we treat every point in 2D space as a point in 3D space, which allows us to represent a transformation using matrices. The 3D point is fixed at 1 so that an (_x_, _y_) point is represented as (_x_, _y_, 1), that is, it does not vary in the third dimension.

In this notation, a point resulting from a transformation, (_p_, _q_), is derived as follows:

![](bbox=[0.4073, 0.5979, 0.5927, 0.6448])

The elements _a_, _b_, _c_, _d_, _e_, and _f_ represent a variety of transformations depending on the values assigned to them.

# # Using transformation matrices

When creating transformations, you can pass matrix parameters directly to a transformation string using the syntax `"M a, b, c, d, e, f"`. This has the effect of applying a transformation matrix to every point on an element.

[ 58 ]