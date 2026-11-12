![](bbox=[0.13, 0.1, 0.87, 0.37])

# The ctree algorithm

The ctree functionÂ gives some advantages over rpart in that the results produced can be a bit more intuitive. Rather than optimizing a result node based purely upon the purity of the resultant node, ctree uses statistical hypothesis testing to determine if the results of the split are statistically significant. It uses chi-square test statistics to test the association, only keeping the associations that are significant, and thus removing bias due to a large number of categories. So, while accuracy may suffer in some cases, benefit is gained by having the results be more explanatory:

```
install.packages("partykit")
library(partykit)
y2 <- ctree(Life.Exp ~ .,data=x)
y2
plot(y2)
```

As you can see from the following plot, ctree and rpart both agree on the first split (murder), and the cutoff points are very similar. This can add additional credence to the first split since the split is corroborated by two different algorithms: