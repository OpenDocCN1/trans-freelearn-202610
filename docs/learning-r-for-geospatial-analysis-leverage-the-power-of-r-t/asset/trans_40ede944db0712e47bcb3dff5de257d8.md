

## Summary

在本章中，你学习了在 R 中使用 ggplot2、ggmap 和 lattice 包进行空间数据高级可视化的一些最有用的方法。展示了如何使用这些工具在 R 环境中完成空间分析流程，并创建可发表的结果地图和图表。在此背景下，指出并非所有事情都能在 R 中完成，有时我们需要迁移到传统的 GIS 软件或图形编辑器，以交互式地定制图形输出。尽管如此，R 中的可视化极其灵活，同时也带来了编程的所有好处。一旦你对本章介绍的技术更加熟悉，R 几乎不可避免地会成为数据可视化的首选工具。我衷心希望，在完成本书后，你对 R 中的地理空间数据分析也有同样的感受。

## A
External Datasets
Used in Examples

本书中的大多数代码示例都使用来自外部文件的数据。为了重现示例，建议你从本书网站下载这些文件，然后将它们复制到硬盘上的单个目录中。在示例代码中，文件假定位于 C:\Data。要使用不同的目录，应相应地修改示例代码。

本书示例中的外部文件，按第一个文件名的字母顺序排列，列在下表中：

| Dataset | Associated files | Description |
|---|---|---|
| Daily meteorological data for Albuquerque International Airport | 338284.csv<br>GHCND_documentation.pdf | 来自美国新墨西哥州阿尔伯克基国际机场气象站的每日气候记录。从 NOAA 气候数据在线网站 http://www.ncdc.noaa.gov/cdo-web 下载。访问时间 2014 年 5 月。 |
| Monthly meteorological data for Spain | 343452.csv<br>GHCNDMS_documentation.pdf | 来自西班牙气象站的月度气候记录。从 NOAA 气候数据在线网站 http://www.ncdc.noaa.gov/cdo-web 下载。访问时间 2014 年 5 月。 |
| US Census data | CO-EST2012-Alldata.csv<br>CO-EST2012-alldata.pdf | 美国人口普查 2012 年县人口变化数据。从美国人口普查局 https://www.census.gov/popest/data/counties/totals/2012/CO-EST2012-alldata.html 下载。访问时间 2014 年 5 月。 |
| Administrative areas of England and Wales | CTYUA_2013_EW_BFE.docx<br>CTYUA_DEC_2013_EW_BFE.dbf<br>CTYUA_DEC_2013_EW_BFE.prj<br>CTYUA_DEC_2013_EW_BFE.sbn<br>CTYUA_DEC_2013_EW_BFE.sbx<br>CTYUA_DEC_2013_EW_BFE.shp<br>CTYUA_DEC_2013_EW_BFE.shp.xml<br>CTYUA_DEC_2013_EW_BFE.shx | 英格兰和威尔士行政区域边界的 Shapefile。从国家统计局 https://geoportal.statistics.gov.uk 下载。访问时间 2014 年 5 月。 |
| GPS log | GPS_log.gpx | 来自 Lahav 和 Dvira 森林一次旅行的 GPS 轨迹记录。从 GPS 设备下载。 |
| OpenStreetMap Shapefile of buildings in Haifa | haifa_buildings.cpg<br>haifa_buildings.dbf<br>haifa_buildings.prj<br>haifa_buildings.qpj<br>haifa_buildings.shp<br>haifa_buildings.shx | 基于 OpenStreetMap 数据的建筑物多边形 Shapefile (http://www.openstreetmap.org)。访问时间 2014 年 5 月。 |
| OpenStreetMap Shapefile of natural areas in Haifa | haifa_natural.cpg<br>haifa_natural.dbf<br>haifa_natural.prj<br>haifa_natural.qpj<br>haifa_natural.shp<br>haifa_natural.shx | 基于 OpenStreetMap 数据的自然区域多边形 Shapefile (http://www.openstreetmap.org)。访问时间 2014 年 5 月。 |
| Portion of Landsat satellite image of central Israel obtained on October 15, 1998 | landsat_15_10_1998.tif | 一个六波段栅格，对应 Landsat 波段 1-5 和 7。图像拍摄于 1998 年 10 月 15 日。从 http://earthexplorer.usgs.gov/ 下载并预处理以获得反射率值。 |
| Portion of Landsat satellite image of central Israel obtained on October 4, 2000 | landsat_04_10_2000.tif | 一个六波段栅格，对应 Landsat 波段 1-5 和 7。图像拍摄于 2000 年 10 月 4 日。从 http://earthexplorer.usgs.gov/ 下载并预处理以获得反射率值。 |
| Portion of Landsat satellite image of central Israel obtained on September 11, 2003 | landsat_11_09_2003.tif | 一个六波段栅格，对应 Landsat 波段 1-5 和 7。图像拍摄于 2003 年 9 月 11 日。从 http://earthexplorer.usgs.gov/ 下载并预处理以获得反射率值。 |
| OpenStreetMap Shapefile of buildings in London | london_buildings.CPG<br>london_buildings.dbf<br>london_buildings.prj<br>london_buildings.shp<br>london_buildings.shx | 基于 OpenStreetMap 数据的建筑物多边形 Shapefile (http://www.openstreetmap.org)。访问时间 2014 年 5 月。 |
| OpenStreetMap Shapefile of natural areas in London | london_natural.CPG<br>london_natural.dbf<br>london_natural.prj<br>london_natural.shp<br>london_natural.shx | 基于 OpenStreetMap 数据的自然区域多边形 Shapefile (http://www.openstreetmap.org)。访问时间 2014 年 5 月。 |
| A portion of NDVI images of central Israel from MODIS satellite data from February 18, 2000 to April 6, 2012 | modis.tif | 一个 280 波段栅格，包含 2000 年 2 月 18 日至 2012 年 4 月 6 日期间的 NDVI 图像。数据来自 MODIS 产品 MOD13A1。从 http://earthexplorer.usgs.gov/ 下载。 |
| Dates of acquisition of images in each layer in modis.tif | modis_dates.csv | 包含 modis.tif 中每个图层的补充信息（日期）的表。 |
| A sample raster file for demonstration in *Chapter 1, The R Environment* | `rainfall.tif` | 以色列北部和中部年降雨量，从气象站数据插值得到。 |
| **Digital Elevation Model (DEM)** layer of Spain | `spain_elev.tif` | 来自 SRTM 数据集的 DEM 数据（参见 http://srtm.csi.cgiar.org/）。通过 R 下载并通过重投影、掩膜和聚合预处理至 900 米分辨率。 |
| GADM Shapefile of the USA counties with FIPS codes | `USA_2_GADM_fips.cpg`<br>`USA_2_GADM_fips.dbf`<br>`USA_2_GADM_fips.prj`<br>`USA_2_GADM_fips.qpj`<br>`USA_2_GADM_fips.shp`<br>`USA_2_GADM_fips.shx` | 对应美国第二级行政区划（县）的多边形，具有县名和 **Federal Information Processing Standards (FIPS)** 代码等多个属性。 |

## B
Cited References

现在我们已经涵盖了所有内容，让我们看一些进一步阅读的外部参考资料：

- Bivand, R. S., Pebesma, E. J., and Gómez-Rubio, V. *Applied Spatial Data Analysis with R Second Edition.* Springer. 2013.
- Chang, W. *R Graphics Cookbook.* O'Reilly Media. 2012.
- Cowpertwait, P. S. P. and Metcalfe, A. V. *Introductory Time Series with R.* Springer. 2009.
- Diggle, P. and Ribeiro, P. J. *Model-based Geostatistics.* Springer. 2007.
- Grolemund, G. and Wickham, H. "Dates and Times Made Easy with lubridate". Journal of Statistical Software. 2011. http://www.jstatsoft.org/v40/i03/paper.
- Hengl, T. "A practical guide to geostatistical mapping of environmental variables". JRC Scientific and Technical Reports. 2007. http://spatial-analyst.net/book/.
- Hengl, T., Roudier, P., Beaudette, D., and Pebesma, E. "plotKML: Scientific Visualization of Spatio-Temporal Data". Journal of Statistical Software. 2014. http://cran.r-project.org/web/packages/plotKML/vignettes/jss1079.pdf.
- Kahle, D. and Wickham, H. "ggmap: Spatial Visualization with ggplot2". The R Journal. 2013. http://journal.r-project.org/archive/2013-1/kahle-wickham.pdf.
- Lamigueiro, O. P. *Displaying Time Series, Spatial, and Space-Time Data with R.* CRC Press. 2014.
- Murrell, P. *Introduction to Data Technologies.* CRC Press. 2009. https://www.stat.auckland.ac.nz/~paul/ItDT/.

