# Visualize Diamond Dataset

![](bbox=[0.105, 0.126, 0.895, 0.616])

There are certain external graphical themes which can be imported to the ggplot2 function for visualization, such as library(ggthemes). Tableau, which is a tool known for data visualization, its color, and themes, can also be used along with ggplots:

```
```r
> #Changing discrete category colors
> ggplot(diamonds, aes(price, carat, color=factor(cut)))+
+ geom_point() +
+ scale_color_brewer(palette="Set1")
> #Using tableau colors
> library(ggthemes)
> ggplot(diamonds, aes(price, carat, color=factor(cut)))+
+ geom_point() +
+ scale_color_tableau()
```
```