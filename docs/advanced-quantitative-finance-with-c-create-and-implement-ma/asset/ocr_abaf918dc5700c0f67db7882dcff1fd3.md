We can arrange the numerical results from our algorithm as shown in the table in the following screenshot, using the transformed variables (upper table) or the original variables (lower table), where we can find that for S=75 and t=0, the option price is £4,20:

| x/tau | -1 | -0,5 | 0 | 0,5 | 1 |
|---|---|---|---|---|---|
| 0,0225 | 0,00 | 0,00 | 0,06 | 0,71 | 1,82 |
| 0,0180 | 0,00 | 0,00 | 0,05 | 0,70 | 1,82 |
| 0,0135 | 0,00 | 0,00 | 0,04 | 0,69 | 1,82 |
| 0,0090 | 0,00 | 0,00 | 0,02 | 0,68 | 1,82 |
| 0,0045 | 0,00 | 0,00 | 0,01 | 0,68 | 1,82 |
| 0,0000 | 0,00 | 0,00 | 0,00 | 0,67 | 1,82 |

| S/t | 27,59 | 45,49 | 75,00 | 123,65 | 203,87 |
|---|---|---|---|---|---|
| 0,0 | 0,00 | 0,15 | 4,20 | 50,49 | 125,93 |
| 0,1 | 0,00 | 0,09 | 3,41 | 50,14 | 126,56 |
| 0,2 | 0,00 | 0,05 | 2,60 | 49,79 | 127,20 |
| 0,3 | 0,00 | 0,02 | 1,76 | 49,42 | 127,84 |
| 0,4 | 0,00 | 0,00 | 0,90 | 49,04 | 128,48 |
| 0,5 | 0,00 | 0,00 | 0,00 | 48,65 | 128,87 |

Example of Finite Difference pricing.

# Summary

In this chapter, we reviewed the basics of the three key numerical methods used to price financial derivatives today. For each of them, we have provided an algorithm and a numerical example. Further, more advanced features of these methods can be found in excellent textbooks by (Glasserman 2003), (Kloeden and Platen 1992), and (Wilmott et al. 1995) as mentioned in all the previously discussed sections.

Not all methods are applicable in all situations, just like the tools in a toolbox. Some methods are more effective to solve some specific problems. For example, with a binomial tree, it is simple to evaluate American options also, while for Monte Carlo, it is not so straightforward. Monte Carlo is more powerful in high-dimensional problems, while finite differences can be used effectively for low-dimensional problems.