```
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

![](bbox=[0.143, 0.381, 0.875, 0.723])