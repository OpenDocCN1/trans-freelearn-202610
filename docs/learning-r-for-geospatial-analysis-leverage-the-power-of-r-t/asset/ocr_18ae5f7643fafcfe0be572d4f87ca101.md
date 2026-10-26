The graphical output is shown in the following screenshot:

![](bbox=[0.25, 0.17, 0.75, 0.56])

The preceding screenshot quite obviously resembles Spain's DEM. In fact, it shows the elevation profile of Spain transposed to temperature units (using a linear relation based on the data from meteorological stations) and locally calibrated using, once again, the stations data. This is what UK results commonly look like.

Let's examine the RMSE of the UK prediction model as well:

```
> cv = gstat.cv(g)
> rmse(cv)
[1] 1.455196
```

We see that the RMSE of UK, in this case, is even lower than that of OK. This result suggests that utilizing the value~elevation trend has further improved prediction accuracy.