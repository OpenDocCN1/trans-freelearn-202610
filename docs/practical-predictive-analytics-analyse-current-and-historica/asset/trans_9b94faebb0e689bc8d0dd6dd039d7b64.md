

| | lm.coef | cat | coef.rank |
|---|---|---|---|
| structure.c..0.406183442737828..0.0003367987... | 0.0003367988 | 18 至 24 岁 | 06 - 18 至 24 岁 |
| structure.c..11.3751875969696..0.00579618220... | 0.0057961822 | 25 至 34 岁 | 23 - 25 至 34 岁 |
| structure.c..10.9168220838485..0.00553403690... | 0.0055340369 | 35 至 44 岁 | 21 - 35 至 44 岁 |
| structure.c..11.5445664482143..0.00582919407... | 0.0058291941 | 45 至 54 岁 | 24 - 45 至 54 岁 |
| structure.c..4.70961214602917..0.00240990753... | 0.0024099075 | 55 至 64 岁 | 14 - 55 至 64 岁 |
| structure.c..1.25623750951236..0.00063341249... | 0.0006334125 | 65 岁及以上 | 07 - 65 岁及以上 |
| structure.c..4.10262144273963..0.00211905813... | 0.0021190581 | 所有年龄段 | 12 - 所有年龄段 |
| structure.c..2.67730000270634..0.00145538791... | 0.0014553879 | 女性 18 至 24 岁 | 09 - 女性 18 至 24 岁 |
| structure.c..9.99097876897398..0.00508800858... | 0.0050880086 | 女性 25 至 34 岁 | 19 - 女性 25 至 34 岁 |
| structure.c..9.5647240414497..0.004850187645... | 0.0048501876 | 女性 35 至 44 岁 | 17 - 女性 35 至 44 岁 |
| structure.c..10.3633655124299..0.00523536960... | 0.0052353696 | 女性 45 至 54 岁 | 20 - 女性 45 至 54 岁 |
| structure.c..4.10277495735534..0.00210834349... | 0.0021083435 | 女性 55 至 64 岁 | 11 - 女性 55 至 64 岁 |
| structure.c..1.36745107793605..0.00068877425... | 0.0006887743 | 女性 65 岁及以上 | 08 - 女性 65 岁及以上 |
| structure.c..3.81748382391257..0.00197005880... | 0.0019700588 | 女性所有年龄段 | 10 - 女性所有年龄段 |
| structure.c..3.26759338613744..0.00157832779... | -0.0015783278 | 女性 18 岁以下 | 02 - 女性 18 岁以下 |
| structure.c..4.03612772673809..0.00186299125... | -0.0018629913 | 男性 18 至 24 岁 | 01 - 男性 18 至 24 岁 |
| structure.c..9.71595028579057..0.00498362081... | 0.0049836208 | 男性 25 至 34 岁 | 18 - 男性 25 至 34 岁 |
| structure.c..7.70662482144783..0.00394154306... | 0.0039415431 | 男性 35 至 44 岁 | 16 - 男性 35 至 44 岁 |
| structure.c..10.9753879171274..0.00554925524... | 0.0055492552 | 男性 45 至 54 岁 | 22 - 男性 45 至 54 岁 |
| structure.c..5.37038054369785..0.00273826914... | 0.0027382691 | 男性 55 至 64 岁 | 15 - 男性 55 至 64 岁 |

## 将分数合并回原始数据框

我们将通过按类别合并，然后按系数排名对数据框进行排序，将此新信息合并回原始 x2 数据框。这样我们就可以将其用作趋势的代理：

```
x2x <- x2 %>% left_join(xx4, by = "cat") %>% arrange(coef.rank, c
```

```
# exclude some columns so as to fit on one page
head(x2x[, c(-2, -3, -4, -8)])
> Source: local data frame [6 x 7]
>
> cat Year.1 Total.People Total Not.Covered.
> (fctr) (int) (dbl) (dbl) (d
> 1 MALE 18 to 24 YEARS 2012 15142.04 11091.86 0.2674
> 2 MALE 18 to 24 YEARS 2011 15159.87 11028.75 0.2725
> 3 MALE 18 to 24 YEARS 2010 14986.02 10646.88 0.2895
> 4 MALE 18 to 24 YEARS 2010 14837.14 10109.82 0.3186
> 5 MALE 18 to 24 YEARS 2008 14508.04 10021.66 0.3092
> 6 MALE 18 to 24 YEARS 2007 14391.92 10230.61 0.2891
> Variables not shown: lm.coef (dbl), coef.rank (fctr)
```

## 使用趋势线绘制数据

现在我们已拥有趋势系数，将首先使用 ggplot 绘制所有 24 个类别的参保人数，然后创建第二组图表，添加基于刚才计算的线性系数的趋势线。

代码说明：facet_wrap 将按变量 z 的值对图表进行排序，该变量被分配为系数排名。因此，我们可以先看到参保人数下降的类别，最后看到 1999-2012 期间参保趋势最高的类别。

### 注意

我喜欢将即将更改的变量分配给标准变量名，例如 x、y 和 z，以便记住它们的用途（例如，变量 x 始终是 x 变量，而 y 始终是 x 变量）。但你也可以直接在 ggplot 调用中提供变量名，或设置自己的函数来完成相同的事情：

```
library(ggplot2)
.df <- data.frame(x = x2x$Year.1, y = x2x$Not.Covered.Pct, z = x2
#use ggplot to layer the different components of the visualization
.plot <- ggplot(data = .df, aes(x = x, y = y, colour = 1, shape =
+ geom_point()
+ scale_shape_manual(values = seq(0, 24))
+ scale_y_continuous(expand = c(0.1,0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z) + xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"), legend.position = "non
+ theme(strip.text.x = element_text(size = 6))
print(.plot)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_469_0.png)

如你所见，对于“所有年龄段”类别，未投保百分比在整个时间段内呈上升趋势，直至 2010 年（《平价医疗法案》颁布之年），之后百分比开始下降。然而，在同一时间段内，18 岁以下年龄组的未投保人口比例相对于人口规模呈下降趋势。

我们可以更仔细地查看前四名和后四名类别，以及“所有年龄段”类别，以更深入地研究这一点。这次，我们将使用 geom_smooth 参数添加自己的趋势线，它将添加线性回归趋势线：

```
# declining enrollment
.df2 <- rbind(head(.df, (4*14)) , tail(.df, (4*14)) , .df[.df$z == "1
.plot2 <- ggplot(data = .df2, aes(x = x, y = y, colour = 1, shape
+ geom_point()
+ scale_shape_manual(values = seq(0, nrow(.df2)))
+ scale_y_continuous(expand = c(0.1, 0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z)
+ xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"),legend.position = "none
+ geom_smooth(method = "lm", se = FALSE,colour = "red")
.plot2 <- ggplot(data = .df2, aes(x = x, y = y, colour = 1, shape
+ geom_point()
+ scale_shape_manual(values = seq(0, nrow(.df2)))
+ scale_y_continuous(expand = c(0.1,0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z)
+ xlab("Year.1")
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme(panel.margin = unit(0.3,"points"),legend.position = "none
+ geom_smooth(method = "lm", se = FALSE,colour = "red")
print(.plot2)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_470_0.png)

## 将所有类别绘制在同一图表上

有时，将所有线条绘制在同一图表上而不是分开的图表会更好。为此，我们会稍微修改语法，使类别显示为堆叠线。同样，我们可以看到不同年龄段的未投保百分比是一致的，其中 18 岁以下组的未投保率最低，而 25-54 岁组的未投保率最高：

```
library(ggplot2)
### plot all on one graph

.df <- x3[order(x3$Year.1), ]
.plot <- ggplot(data = .df, aes(x = Year.1, y = Not.Covered.Pct,
+ geom_point()
+ geom_line(size = 1)
+ scale_shape_manual(values = seq(0,15))
+ ylab("Not.Covered.Pct")
+ labs(colour = "cat", shape = "cat")
+ theme_bw(base_size = 14,base_family = "serif")
print(.plot)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_472_0.png)

## 添加标签

有时，根据图例辨别类别之间的差异可能有些困难，尤其是类别数量增加时，因此我们可以使用 directlabels 库为每条线标记类别名称。现在我们可以看到，例如，所有男性类别在所有时间段的未投保率均高于所有女性类别：

```
#install.packages("directlabels")
library(directlabels)
direct.label(.plot, list(last.points, hjust = 0.75, cex = 0.75, v
```

Not.Covered.Pct

Year.1

- 25 至 34 岁
- 35 至 44 岁
- 45 至 54 岁
- 男性所有年龄段
- 所有年龄段
- 女性所有年龄段
- 女性 19 岁以下
- 65 岁及以上

![](img/672c0cf8ba3c000a6529b117ed96945e_473_0.png)

