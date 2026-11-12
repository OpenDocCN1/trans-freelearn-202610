+   6. Double-click on Tab Avg, Tab Median, and Tab Stdev. They will now appear on the Measures Values shelf:

![](bbox=[0.091, 0.116, 0.866, 0.573])

# Figure 14.10: Testing R

+   7. Make sure R is installed, connected, and running as per the instructions in the R installation and integration section

+   8. If you haven't installed the Rserve package yet, type install.packages("Rserve") into your R interface to install the Rserve package

+   9. Next, type library(Rserve); Rserve() into the R interface. You may see the following error:

```
Fatal error: you must specify '--save', '--no-save' or '--vanilla'
```

In this case, type Rserve(args = "--no-save"). R requires you to make a choice of saving, not saving, or a combination (vanilla) after your session ends (not saving is my preferred option but the other two will work too).