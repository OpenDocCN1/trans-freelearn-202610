We find the eigenvalues and eigenvectors:

```
In [13]: %octave mat = [1 2 3; 4 5 6; 7 8 9]; [vect, vals] = eig(mat)

vect =

   -0.231971  -0.785830   0.408248
   -0.525322  -0.086751  -0.816497
   -0.818673   0.612328   0.408248

vals =

Diagonal Matrix

    1.6117e+01             0             0
             0   -1.1168e+00             0
             0             0   -1.3037e-15
```

# Tricky issues

Note that the syntax/semantics can be tricky here. The definition of the `eig` function in the Octave language specifies the following behavior:

+   - `lambda = eig(A)` returns the eigenvalues of A in the vector lambda.
+   - `[V, lambda] = eig(A)` also returns the eigenvectors in V, but lambda is now a matrix whose diagonals contain the eigenvalues. This relationship holds true (within round-off errors): `A = V*lambda*inv(V)`.

The `[a, b]` notation and multiple return values do not carry over cleanly into IPython.

So, this sort of works as expected (one might as well expect x to be a two-element vector):

```
In [14]: x = %octave mat = [1 2 3; 4 5 6; 7 8 9]; eig(mat)

ans =

    1.6117e+01
   -1.1168e+00
   -1.3037e-15

In [15]: x
```