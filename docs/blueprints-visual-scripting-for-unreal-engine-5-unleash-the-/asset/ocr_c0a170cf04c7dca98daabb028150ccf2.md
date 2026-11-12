![](bbox=[0.15, 0.1, 0.55, 0.4])

# Figure 14.13 – Adding vectors

For example, if v1 is the character's world location and v2 is the relative location of the character's weapon, then v1 + v2 will be the weapon's world location.

The following screenshot shows the vector addition node:

![](bbox=[0.12, 0.61, 0.8, 0.7])

# Figure 14.14 – Node to add vectors

+   Vector subtraction: The subtraction of one vector from another is determined by subtracting each of its elements. This is an example of vector subtraction using the vectors (6, 8, 0) and (1, 4, 0):

```
v1 = (6, 8, 0) v2 = (1, 4, 0) v1 - v2 = (6 - 1, 8 - 4, 0 -
0) v1 - v2 = (5, 4, 0)
```