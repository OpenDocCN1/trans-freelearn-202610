A norm of a vector characterizes its length. There is a special class of vectors with the property that are shown in the following equation:

These vectors are called **unit vectors.**

+   - **Orthogonality**: The vectors have another property called **orthogonality**. Two vectors, $x$ and $y$, are said to be **orthogonal** if their inner product is zero. Thus, vectors $x$ and $y$ are orthogonal if $\langle x,y\rangle = 0$. Geometrically, orthogonal vectors are perpendicular. Vectors that are both orthogonal and are unit vectors (have a length of 1) are said to be **orthonormal.**

# Understanding the properties and types of matrices

A **matrix** is defined as a two-dimensional array of scalars, with at least one row and at least one column. An $n\times m$ matrix $A$ is defined as a two-dimensional array with $n$ rows and $m$ columns, and is given as:

$A_{n\times m} = \begin{bmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ \vdots & \vdots & \cdots & \vdots \\ a_{n1} & a_{n2} & \cdots & a_{nn} \end{bmatrix}.$

Now, let's discuss the properties of a matrix:

+   - **Determinant**: One property of a matrix is its determinant. Determinants of matrices are only defined for square matrices, where the number of rows is the same as the number of columns. The determinant of a matrix $A$, denoted by $det(A)$ or $|A|$, is a scalar.

The Python code snippet for computing the determinant of a matrix is given as follows:

```
import numpy as np

A = np.array([[1,2,3], [4,5,6], [1,5,1]]) # a 3x3 matrix.
det_A = np.linalg.det(A)
print(det_A)
```

The preceding code uses the linalg.det() function from numpy in order to compute the determinant of the matrix A:

+   - **Trace**: Matrices also have a property called trace. For a matrix $A$, the trace of $A$, denoted by $T(A)$, is given as a sum of the diagonal elements of $A$.