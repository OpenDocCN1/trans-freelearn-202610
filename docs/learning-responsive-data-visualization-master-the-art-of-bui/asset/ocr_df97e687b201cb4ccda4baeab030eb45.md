One could have the great idea of copying the whole 3D sphere into a plane—like in the 3D world map figure in the first section. However, we only see one side of the sphere here; and hence, we loose all the information about the other side.

This mapping of a sphere to a plane is called projection. D3 offers a variety of built-in projections for all kinds of mappings, available under the d3.geo namespace.

> > You can find more information about maps on Mike Bostock's blog at http://bost.ocks.org/mike/map/.

# # The Equirectangular projections

Remember the brain experiment we just did before? To create such a simple projection in D3, we can use the d3.geo.equirectangular() function that preserves neither distances nor areas. It is the most basic projection because parallels and meridians are simply mapped to parallels and normals in a Euclidean space. We can see the previously discussed distortion effects in the following figure:

![](bbox=[0.054, 0.418, 0.895, 0.73])

Due to the fact of not preserving areas, angles, or distances, it is rarely used for visualizing geographic features. However, it is used sometimes for raster reprojection of pixel values to other representations.