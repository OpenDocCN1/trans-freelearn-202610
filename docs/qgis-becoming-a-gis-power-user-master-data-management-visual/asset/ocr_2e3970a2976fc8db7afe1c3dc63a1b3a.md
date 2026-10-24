Now that all our data is in the raster format, we can work through how to derive information from these layers and combine this information in order to select the best sites.

# Map algebra

Map algebra is a useful concept to work with multiple raster layers and analysis steps, providing arithmetic operations between cells in aligned grids. These produce an output grid with the respective value of the arithmetic solution for each set of cells. We will be using map algebra in this example for additive modeling.

# Additive modeling

Now that all our data is in the raster format, we can begin to model for the purpose of site selection. We want to discover which cells are best according to a set of criteria which has either been established for the domain area (for example, the agricultural conservation site selection) by convention or selected at the time of modeling. Additive modeling refers to this process of adding up all the criteria and associated weights to find the best areas, which will have the greatest value.

In this case, we have selected some criteria that are loosely known to affect the agricultural conservation site selection, as shown in the following table:

| Layer | Criteria | Rule |
|---|---|---|
| applicants | Is applicant | |
| easements | Proximity | < 2000 m |
| landuse (agriculture) | Land use, proximity | < 100 m |
| dem | Slope | >= 2 and <= 5, average |
| landuse (developed) | Land use, proximity | > 500 m |
| roads | Proximity | > 100m |

# Proximity

The Proximity grid tool will generate a layer of cells with each cell having a value equal to its distance from the nearest non-nodata cell in another grid. The distance value is given in the CRS units of the other grid. It also generates direction and allocation grids with the direction and ID of the nearest nodata cell.