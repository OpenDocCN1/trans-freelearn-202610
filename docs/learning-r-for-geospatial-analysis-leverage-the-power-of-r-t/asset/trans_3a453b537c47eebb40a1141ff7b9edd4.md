

与聚合相反的操作是解聚；可以使用 disaggregate 函数来执行。例如，表达式 `disaggregate(dem_agg8,fact=8)` 会生成一个与 dem 分辨率相同的更高分辨率栅格。然而，聚合中丢失的信息无法恢复（每组 8*8 相邻像元将具有相同的值），并且解聚后的栅格在视觉上看起来与 dem_agg8 完全相同。

## 栅格重采样与重投影

在本节中，我们将把给定栅格的值从其自身网格传输到另一个网格。此操作称为重采样。栅格重投影是一个紧密相关的操作；它基本上是将栅格重采样到具有不同于原始栅格 CRS 的网格上。

## 栅格重采样

可以使用 `resample` 函数执行栅格重采样。该函数的必需参数如下：

- 要对其值进行重采样的栅格 (x)
- 定义值将被传输到的网格的栅格 (y)
- 重采样方法 (`method`)

`resample` 函数目前提供两种重采样方法。我们使用的方法决定了新栅格中的像元根据旧栅格的值获取其值的方式：

- `"ngb"`：最近邻；分配最近像元的值
- `"bilinear"`：双线性插值；分配四个最近像元的加权平均值（默认）

![](img/ebfd6294770b225fdeceda9a7b8b560e_200_0.png)

最近邻插值保留原始值，仅根据邻近关系将它们传输到新网格，而使用多个像元加权平均的重采样方法（如双线性插值）不可避免地会修改原始值，从而生成更平滑的图像。因此，在需要保留原始值的情况下，建议使用最近邻重采样方法。例如，在整数表示不同覆盖类型的分类栅格中，计算类别的加权平均值是没有意义的。例如，在一个具有两个类别（比如1代表森林，2代表牧场）的栅格中，四个像元值的平均值通常既不等于1也不等于2，这使得生成的图像毫无意义。另一方面，当视觉外观是主要考虑因素时，建议使用双线性插值和类似的加权平均方法。

对于我们的重采样示例，我们将使用两幅 NDVI 影像：一幅来自 Landsat，另一幅来自 MODIS。由于 Landsat 和 MODIS 影像定义在相同的 CRS 中且彼此重叠（MODIS 影像范围涵盖 Landsat 影像范围）——尽管它们的分辨率差异很大（Landsat 影像分辨率为 30 m，而 MODIS 分辨率为 500 m）——我们可以用其中一个对另一个进行重采样。在接下来的三个示例中，我们将执行以下步骤：

- 1. 将原始 Landsat 影像与根据 MODIS 网格重采样后的影像进行比较。
- 2. 将原始 MODIS 影像与根据 Landsat 网格重采样后的影像进行比较。
- 3. 使用两种方法比较 MODIS 影像的重采样结果：最近邻和双线性插值。

对于 Landsat NDVI 影像，我们将使用之前基于 2000 年 10 月 4 日获取的影像计算得到的 `ndvi_00` 栅格。至于 MODIS，栅格 `r` 包含 2000 年 2 月 18 日至 2012 年 4 月 6 日期间的 280 幅 NDVI 影像（参见*第 4 章，使用栅格*）。为了使比较更有意义，我们将选择最接近 2000 年 10 月 4 日的获取日期。使用 MODIS 获取日期的 `dates$date` 向量（正如我们在*第 3 章，使用表格*中看到的），我们可以通过执行以下步骤找出最接近 2000 年 10 月 4 日的日期的索引：

- 1. 计算 `dates$date` 中每个日期与 2000 年 10 月 4 日之间的差值向量。
- 2. 使用 `abs` 函数计算绝对差值。
- 3. 使用 `which.min` 找到最小差值对应的索引。

这三个步骤可以用一行代码完成，如下所示：

```
> l_date = which.min(abs(dates$date - as.Date("2000-10-04")))
```

结果赋值给 l_date，它是 MODIS 影像中获取日期最接近 2000 年 10 月 4 日的索引，等于 15：

```
> l_date
[1] 15
```

因此，在示例中，我们将使用 r[[l_date]]，即多波段栅格 r 的第十五个图层（相应的获取日期是 2000 年 9 月 29 日，如 dates$date[l_date] 所示）。

在我们的第一个示例中，我们将使用最近邻方法将 Landsat 影像 ndvi_00 重采样到 r[[l_date]] 影像的 MODIS 网格，如下所示：

```
> l_resample = resample(ndvi_00, r[[l_date]], method = "ngb")
```

让我们通过绘图来比较原始 Landsat 影像 ndvi_00 和重采样影像 l_resample，如下所示：

```
> plot(ndvi_00, main = "Original Landsat image")
> plot(l_resample, ext = extent(ndvi_00),
+ main = "Resampled to MODIS")
```

注意，在绘制 l_resample 时，我们为 ext 参数提供了原始 Landsat 影像 ndvi_00 的范围，以放大重叠区域。实际上，l_resample 影像比 ndvi_00 更大（其范围与 r[[l_date]] 相同），但所有不与 ndvi_00 重叠的像元都被赋值为 NA。

得到的两幅影像如下图所示：

![](img/ebfd6294770b225fdeceda9a7b8b560e_202_0.png)

在左侧，我们可以看到细节丰富的 30 米分辨率 ndvi_00 影像；在右侧，我们看到粗糙的 500 米分辨率影像，其中 ndvi_00 最近像元的值被分配给 r[[l_date]] 中的每个像元。

![](img/ebfd6294770b225fdeceda9a7b8b560e_203_0.png)

> 当原始栅格比重采样结果细致得多时（如前面的示例），会发生相当大的数据丢失。这是因为在新图像中，只有一个（最近邻）或少数几个（双线性插值）30 米像元的值决定了更大的 500 米像元的值，在我们这个特定示例中就是如此。当重采样导致栅格分辨率大幅降低时，按多边形进行聚合和分区提取（我们将在下一章中看到）是更理想的解决方案。

相反的操作是将 MODIS NDVI 值从 r[[l_date]] 重采样到 ndvi_00 的 Landsat 网格，同样使用最近邻方法：

```
> r_resample = resample(r[[l_date]], ndvi_00, method = "ngb")
```

在这种情况下，结果 r_resample 的范围比原始 MODIS 影像 r[[l_date]] 小；其范围与 ndvi_00 相同。为了更方便地将它们并排显示，我们首先将 r_resample 扩展到与 r[[l_date]] 的范围匹配。这可以使用 extend 函数来完成，该函数根据一个 Extent 对象（或可从中派生 Extent 对象的对象，如 RasterLayer）扩展栅格。extend 函数会根据需要添加 NA 行和列以增大栅格范围；因此，从某种意义上说，它是 trim 的反向操作：

```
> r_resample = extend(r_resample, r[[l_date]])
```

现在我们将绘制第二个重采样示例的结果：原始 MODIS 影像 r[[l_date]] 和重采样影像 r_resample。在绘制 r[[l_date]] 时，我们将添加一个围绕 ndvi_00 范围的矩形以帮助比较：

```
> plot(r[[l_date]], main = "Original MODIS image")
> plot(extent(ndvi_00), add = TRUE)
> plot(r_resample, main = "Resampled to Landsat")
```

## 修改栅格和分析栅格时间序列

得到的两幅影像如下图所示：

![](img/ebfd6294770b225fdeceda9a7b8b560e_204_0.png)

正如我们所见，重采样影像在视觉上与原始影像的相应部分相同，尽管其分辨率为 30 米，而原始影像分辨率为 500 米。原因是，与 MODIS 影像中单个 500 米像元重合的每组 30 米 Landsat 像元都会获得该单个像元的值。因此，即使底层分辨率高得多，看起来仍像拥有 500 米像元。

如果我们想让图像看起来更平滑该怎么办？在这种情况下，我们可以使用双线性插值方法而不是最近邻方法。让我们将上一个示例中的重采样操作执行两次，分别使用最近邻方法（与我们刚才所做的方式相同，这次将结果赋值给 r_resample_ngb）和双线性插值方法（将结果赋值给 r_resample_bil）。然后将结果合并到一个双波段栅格中，图层名称指定重采样方法：

```
> r_resample_ngb = resample(r[[l_date]], ndvi_00, method = "ngb")
> r_resample_bil = resample(r[[l_date]], ndvi_00,
+ method = "bilinear")
> resample_results = stack(r_resample_ngb, r_resample_bil)
> names(resample_results) = c("Nearest neighbor",
+ "Bilinear interpolation")
```

