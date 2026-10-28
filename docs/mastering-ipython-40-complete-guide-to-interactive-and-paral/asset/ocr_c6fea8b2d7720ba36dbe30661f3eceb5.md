# Works Well with Others – IPython and Third-Party Tools

```
```text
<FloatVector - Python:0x2ad3d0e06508 / R:0x2847ca0>

[4.000000, 4.000000, 4.000000, ..., 5.000000, 5.000000, 4.000000]

disp: <class 'rpy2.robjects.vectors.FloatVector'>

<FloatVector - Python:0x2ad3d30e06a08 / R:0x3bc0b60>

[4.000000, 4.000000, 1.000000, ..., 6.000000, 8.000000, 2.000000]
```
```

For the most part, everything that can be done with an Rmagic can be done with robjects.r:

```
```python
In [18]: %R

   ....: letters
   ....:

[1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
[1] "r" "s"
[20] "t" "u" "v" "w" "x" "y" "z"
```
```

```
```python
In [19]: robjects.r.letters

Out[19]:

<StrVector - Python:0x2ad3d8887f48 / R:0x4735690>
['a', 'b', 'c', ..., 'x', 'y', 'z']
```
```

However, the correspondence is not exact, as this attempt to access the wt column of the mtcars data frame will demonstrate:

```
```python
In [8]: robjects.r.mtcars$mpg
   File "<ipython-input-8-d46a62a01451>", line 1
     robjects.r.mtcars$mpg
                      ^
SyntaxError: invalid syntax
```
```

A numeric index is required:

```
```python
In [17]: robjects.r.mtcars[5]

Out[17]:

<FloatVector - Python:0x2ad3d887efc8 / R:0x31b0650>
[2.620000, 2.875000, 2.320000, ..., 2.770000, 3.570000, 2.780000]
```
```