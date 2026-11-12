# Understanding EXCLUDE

Let's start understanding the EXCLUDE LOD. The EXCLUDE LOD will exclude any dimension to which it is directed. If your view contains **Region** and **Category** and you write an EXCLUDE LOD on **Category**, only **Region** will be considered in the calculations. The following example will make this clearer—our goal in this exercise is to calculate SUM(Sales) by using fewer dimensions than in the view. We want to exclude visible dimensions only from the calculation but still show the dimension values in the dashboard:

+   1. Set the parameters as shown in the following screenshot:

![](bbox=[0.386, 0.246, 0.619, 0.769])

Figure 7.18: Fixed: Nothing