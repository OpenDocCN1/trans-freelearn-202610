A common source of error is forgetting to add the file format extension to the Output file path (in our example, .tif for GeoTIFF). Similarly, you can get errors if you try to overwrite an existing file. In such cases, the best way to fix the error is to either choose a different filename or delete the existing file first.

The resulting layer will be loaded automatically, since we have enabled the Load into canvas when finished option. QGIS should also automatically recognize the alpha layer that we created, and the raster areas that fall outside the Alaska landmass should be transparent, as shown on the right-hand side in the previous screenshot. If, for some reason, QGIS fails to automatically recognize the alpha layer, we can enable it manually using the Transparency band option in the Transparency section of the raster layer's properties, as shown in the following screenshot. This dialog is also the right place to specify any No data value that we might want to be used:

![](bbox=[0.18, 0.39, 0.82, 0.57])

# Analyzing elevation/terrain data

To use terrain analysis tools, we need an elevation raster. If you don't have any at hand, you can simply download a dataset from the NASA Shuttle Radar Topography Mission (SRTM) using http://dwtkns.com/srtm/ or any of the other SRTM download services.

If you want to replicate the results in the following exercise exactly, then get the dataset called srtm_05_01.zip, which covers a small part of Alaska.