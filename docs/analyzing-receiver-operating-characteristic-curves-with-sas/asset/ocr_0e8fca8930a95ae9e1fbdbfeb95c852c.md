The output from this invocation of PROC NLMIXED appears here. The parameter estimates for α, β, and σ² are −0.241, 1.254, and 0.9386, with standard errors 0.148, 0.197, and 0.1327. The estimates for α and β are identical to the ones provided by PROC GLM (up to rounding), but the estimates for σ² seem to differ slightly (0.9578 vs. 0.9386). This discrepancy results from the different estimation methods used by the two procedures. It is usually small, especially with moderate to large sample sizes. We will discuss this next.

# Output A.2

```
Simple Linear Model Using NLMIXED

The NLMIXED Procedure

Specifications

Data Set                          WORK.APPENDIX
Dependent Variable                 Y
Distribution for Dependent Variable Normal
Optimization Technique             Dual Quasi-Newton
Integration Method                 None

Dimensions

Observations Used                  100
Observations Not Used               0
Total Observations                100
Parameters                          3

Parameters

var    alpha    beta    NegLogLike
1          0       0      168.83132

Iteration History

Iter    Calls    NegLogLike      Diff      MaxGrad      Slope
1          2    148.184071   20.64725   18.21043   -535.042
2          3    142.074894    6.109177  10.23585    -21.7239
3          6    140.056178    2.018716   7.234336     -8.129
4          7    139.602406    0.453772   9.640041    -4.14434
5          8    138.860921    0.741485   3.650731    -1.86665
6         10    138.744238    0.116683   1.583923    -0.32313
7         12    138.727766    0.016472   0.114029    -0.03642
8         14    138.727636    0.00013    0.008173    -0.00027
```