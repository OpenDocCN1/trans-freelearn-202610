Let’s create a function that defines the Vietoris–Rips simplicial complex for two-way interactions (corresponding to a network) using Script 6.2:

```
#define Vietoris-Rips complex
from itertools import combinations
from numpy import linalg as LA
def graph_VR(points, eps):
    points=[np.array(x) for x in points]
    vr=[(x,y) for (x,y) in combinations(points, 2)
    if LA.norm(x - y) <= 2*eps]
    return np.array(vr)
```

Now, we can apply this to the first slice of our stock market time series data. We’ll choose thresholds of 1 and 10 as a starting point to understand which vertex pairs will be included in our simplicial complex (here, only at the two-way interaction level). Let’s add this to Script 6.2 to calculate the Vietoris–Rips simplicial complex for the first slice of our stock market time series data:

```
#apply Vietoris-Rips with multiple thresholds to a slice of our stock
#dataset
data=mydata.iloc[0:5,1:5]
vr1=graph_VR(data.transpose(),1)
vr2=graph_VR(data.transpose(),10)
```

We can examine the vertex pairs included in each filtration by adding the following to Script 6.2:

```
#print the results
print("Vietoris-Rips Complex, Threshold=1:")
print(vr1)
print("Vietoris-Rips Complex, Threshold=10:")
print(vr2)
```

This gives us pairs of vertices for each filtration, which should show the following:

```
Vietoris-Rips Complex, Threshold=1:
[[0 1]
 [0 2]
 [1 2]
 [1 3]
 [2 3]
 [2 4]
 [3 4]]
Vietoris-Rips Complex, Threshold=10:
[[0 1]
 [0 2]
 [0 3]
```