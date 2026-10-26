The mask argument can either be an overlapping raster (in which case the values in x corresponding to NA in mask are assigned with NA) or a vector layer (in which case the values in x not coinciding with any feature in mask are assigned with NA). Therefore, the following expression yields a new raster based on slope where all pixels not covered by haifa_natural are masked (that is, assigned with NA):

```
> natural_mask = mask(slope, haifa_natural)
```

> The previous expression is analogous to the following expression:
> > natural_mask = mask(slope,
> + rasterize(haifa_natural, slope))

For convenience, we will crop the result, natural_mask, using haifa_ext:

```
> natural_mask = crop(natural_mask, haifa_ext)
```

We will repeat the exact same procedure with haifa_buildings to get the buildings_mask raster as well:

```
> buildings_mask = mask(slope, haifa_buildings)
> buildings_mask = crop(buildings_mask, haifa_ext)
```

Now let's plot both natural_mask and buildings_mask, side-by-side, to observe how masking has been carried out:

```
> plot(stack(natural_mask, buildings_mask))
```

The resulting graphical output is shown in the following screenshot:

![](bbox=[0.215, 0.575, 0.785, 0.765])