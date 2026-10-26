

检查表格还会发现，除了县级条目外，它还包含整个州的小计条目。这些条目在县列中的值为 0。由于我们感兴趣的是县，而不是州，因此需要删除这些条目：

```
> dat = dat[dat$county != 0, ]
```

在完成第一步和第二步后，我们将创建一个用于县图层和 dat 表格连接的键。最不麻烦的选择是使用 FIPS 代码作为键，因为不同数据集中的县名可能略有不同。

县图层中我们已经有 FIPS 列。例如，前 10 个要素的 FIPS 代码如下：

```
> county$FIPS[1:10]
  [1] "09005" "09003" "09013" "09015" "06093" "06015" "06049"
  [8] "09011" "09001" "09007"
```

这些五位代码包含州代码（第 1 到 2 位）和县代码（第 3 到 5 位）。正如我们所见，当州代码或县代码少于两位或三位时，其前面会加上零。例如，第一个县多边形的 FIPS 代码为 09005，这意味着其州代码为 9（康涅狄格州）而县代码为 5（利奇菲尔德县）。然而，在 dat 表格中，州和县的 FIPS 代码被保留为没有前导零的单独数值（9 和 5）。为了使值与县图层匹配，我们需要执行以下步骤：

+   1. 在州列中添加前导零以获得统一的两位代码。
+   2. 在县列中添加前导零以获得统一的三位代码。
+   3. 将州列和县列粘贴在一起以获得 FIPS 代码。

前两步可以使用 formatC 函数执行。该函数可以处理多种格式化任务，包括我们需要的任务——用前导零填充向量的值，以获得统一的字符长度。在这种特定情况下，我们需要指定三个参数：要操作的值向量、所需的最终字符数（width 参数）以及格式修饰符“0”（flag 参数，其中“0”表示用前导零填充的场景；参见 ?formatC）：

```
> dat$state = formatC(dat$state, width = 2, flag = "0")
> dat$county = formatC(dat$county, width = 3, flag = "0")
```

我们可以检查修改后的州和县列的前几个值，以确保获得了预期结果：

```
> dat$state[1:10]
[1] "01" "01" "01" "01" "01" "01" "01" "01" "01" "01"
> dat$county[1:10]
[1] "001" "003" "005" "007" "009" "011" "013" "015" "017" "019"
```

请注意，数值已被自动转换为字符，因为数值不能有前导零。

最后，我们将州和县代码粘贴到县 FIPS 代码中，并将其分配给名为 FIPS 的新列：

```
> dat$FIPS = paste0(dat$state, dat$county)
```

现在，dat$FIPS 中的格式与 county$FIPS 中看到的格式相同：

```
> dat$FIPS[1:10]
[1] "01001" "01003" "01005" "01007" "01009" "01011" "01013"
[8] "01015" "01017" "01019"
```

我们准备进入第四步——将县的属性表与 dat 表格连接。事实上，我们将使用 dat 的一个子集，其中只包含两列：FIPS（因为连接将基于它）和 census2010pop（因为这些是我们感兴趣的数据），因为州和县列在此时对我们没有用处。连接操作使用 plyr 包中的 join 函数完成（参见第 3 章，Working with Tables）：

```
> library(plyr)
> county@data = join(county@data,
+ dat[, colnames(dat) %in% c("FIPS", "census2010pop")],
+ by = "FIPS")
```

> 使用 county@data 作为 join 中的第一个参数以及正在使用的连接类型（默认为“left”），可确保 county@data 中的所有条目都按其原始顺序保留（无论它们在 dat 中是否有匹配项）。这极其重要，因为 county@data 的行对应于县多边形。因此，更改行的顺序会导致属性表与空间要素之间出现不一致。关于矢量图层，使用 join(county@data, x, type="left") 之类的操作是安全的，而手动修改 @data 组件涉及更改行顺序的操作（例如删除单行）通常应避免。

结果，我们现在在县的属性表中有了匹配的 census2010pop 条目：

```
> head(county@data)
      NAME_1      NAME_2 TYPE_2  FIPS      area census2010pop
1 Connecticut Litchfield County 09005 2451.876      189927
2 Connecticut   Hartford County 09003 1941.110      894014
3 Connecticut    Tolland County 09013 1077.789      152691
4 Connecticut    Windham County 09015 1350.476      118428
5 California   Siskiyou County 06093 16416.572      44900
6 California   Del Norte County 06015 2626.707      28610
```

我们可以检查并发现，县图层中只有一个条目无法与 dat 中的 census2010pop 条目匹配（因此 census2010pop 列中为 NA）。该条目对应于弗吉尼亚州的 Clifton Forge City：

```
> county@data[is.na(county$census2010pop),
+ c("NAME_1", "NAME_2")]
      NAME_1          NAME_2
2591 Virginia Clifton Forge City
```

我们的第五步也是最后一步将是计算每个县的人口密度，通过将人口规模（census2010pop 列）除以县面积（area 列）。结果可以分配给县属性表中的一个新列，命名为 density：

```
> county$density = county$census2010pop / county$area
```

检查属性表显示，新的列，即每平方公里平均人口密度，确实已添加到县图层中：

```
> head(county@data)
      NAME_1      NAME_2 TYPE_2  FIPS      area census2010pop
1 Connecticut Litchfield County 09005 2451.876      189927
2 Connecticut   Hartford County 09003 1941.110      894014
3 Connecticut    Tolland County 09013 1077.789      152691
4 Connecticut    Windham County 09015 1350.476      118428
5 California   Siskiyou County 06093 16416.572      44900
6 California   Del Norte County 06015 2626.707      28610
density
1 77.461920
2 460.568482
3 141.670638
4 87.693548
5 2.735041
6 10.891965
```

准备县人口密度地图的工作推迟到第 9 章，Advanced Visualization of Spatial Data 时再进行。

## Summary

在本章中，我们介绍了在 R 中处理矢量图层涉及的基本操作。首先，我们回顾了在 R 中表示空间矢量图层的类，并探讨了两种将空间矢量数据引入 R 的方法（地理编码和从文件读取）。其次，我们讨论了如何检查和修改矢量图层的属性表，如何根据其属性表创建图层的子集，以及如何将新数据连接至属性表（来自单独的表或来自另一个矢量图层）。第三，介绍了矢量图层的几何相关操作的主要类型，包括计算几何属性、评估图层之间的关系以及基于现有图层创建新图层。

在下一章中，我们将更详细地探讨栅格，检查栅格的几何相关修改（例如重投影）、利用高程栅格（例如 DEM）以及处理时空栅格数据（例如卫星图像的时间序列数据）。稍后，在第 7 章，Combining Vector and Raster Datasets 中，我们将讨论与栅格和矢量数据之间的相互关系相关的几个常见程序。

## 6
Modifying Rasters and
Analyzing Raster Time Series

在本章中，我们将继续介绍 *Chapter 4, Working with Rasters* 中介绍的材料，转向更高级的操作。这些操作涉及栅格几何属性的修改或栅格上的方向和距离相关计算；主要使用 `raster` 包中的附加函数。本章最后一节将介绍与时空遥感数据分析相关的示例，以演示如何构建更复杂、定制的栅格处理程序。

在本章中，我们将使用我们在 *Chapter 3, Working with Tables* 和 *Chapter 4, Working with Rasters* 中之前创建的对象。

在本章中，我们将涵盖以下主题：

+   - Modifying the geometry of raster layers
+   - Applying focal filters
+   - Clumping patches of connected cells
+   - Resampling and reprojection of rasters
+   - Performing topography-related calculations on elevation data
+   - Aggregating spatio-temporal raster data

