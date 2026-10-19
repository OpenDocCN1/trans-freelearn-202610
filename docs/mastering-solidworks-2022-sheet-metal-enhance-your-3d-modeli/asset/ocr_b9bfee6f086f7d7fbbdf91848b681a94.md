As the gauge number is changed, you'll notice that the **Bend Radius** value also automatically changes to correspond to the new sheet sizes. As expected, thicker sheets require a higher **Bend Radius** value.

> **Exercise Caution When Using Gauge Numbers!**
> Gauge numbers are specific to the material used, so confusion is easily caused. For example, a given gauge number of steel is **not the same thickness** as one in aluminum. **16 Gauge** steel is 1.52mm thick, but **16 Gauge** aluminum is only 1.29mm thick. For this reason, many international standards organizations recommend against the use of gauge numbers and instead suggest specifying the sheet thickness using dimensions.
> 
> The Gauge Tables can give a good indication of bend radii, but I recommend using them as a guide only, and instead directly setting and specifying sheet thicknesses.

The **Bend Radius** is a vital *aspect* of SolidWorks Sheet Metal and is needed for every bent *part*. It can be set in the **Sheet Metal** properties as a global value, but can also be overridden for certain bends if needed.

Get an idea of the **Bend Radius** needed by either speaking to your manufacturer, searching online, or using **Gauge Tables**. However, if using gauge numbers and tables, be careful to ensure your material choice is correct and clearly marked on any drawings. It is wise to use gauge tables as a guide only, and instead (or in addition to this) specify an exact sheet thickness.

# Understanding the Bend Allowance

The next Sheet Metal property that affects your models is the **Bend Allowance** property (labeled *iv* in *Figure 1.8*). This allows us to accurately calculate the size of bent *parts* once they have been unfolded into a flat sheet.