7. Then, create a calculated field called Random Normal Param like:

```
SCRIPT_REAL("
from numpy import random as rd
mu, sigma = _arg2, _arg3
return (rd.normal(mu, sigma, _arg1[0])).tolist()
",
SIZE(), [mu], [sigma]
)
```

8. Add the parameter control to your view so your users can decide which variables they want to pass to Python:

![](bbox=[0.185, 0.305, 0.825, 0.565])

Figure 14.35: Final normal distribution worksheet

Compared to our prior visualization with the normal distribution, you can see in the preceding screenshot that by changing the mu parameter to 10, we change the mean to 10 and thus can move the whole dot cloud in any direction. With the random number being available in Tableau, you can, for example, visualize a Monte Carlo simulation, which is used in many real-life applications because it can model possible probability outcomes and thus helps interested parties understand the impact of risk and uncertainty.

> **Note**
> 
> More information on how to calculate a Monte Carlo simulation in Tableau can be found here -- https://jacksontwo.com/exploring-python-tableau – and more general Monte Carlo content can be found here: https://www.ibm.com/topics/monte-carlo-simulation.