# Output 7.1

The SAS System
The LIFETEST Procedure

# Product-Limit Survival Estimates

| Timelist | Survival | Survival | Failure | Survival Standard Error | Number Failed | Number Left |
| 36.0000 | 36.0000 | 0.7676 | 0.2324 | 0.0529 | 19 | 23 |

# Summary Statistics for Time Variable Survival

# Quartile Estimates

| Percent | Point Estimate | 95% Confidence Interval [Lower Upper) | |
| 75 | . | . | . |
| 50 | . | 49.0000 | . |
| 25 | 44.0000 | 24.0000 | . |

Mean    Standard Error
41.8379        1.4697

NOTE: The mean survival time and its standard error were underestimated because the largest observation was censored and the estimation was restricted to the largest event time.

# Summary of the Number of Censored and Uncensored Values

| Total | Failed | Censored | Percent Censored |
| 100 | 21 | 79 | 79.00 |

# 7.3.2 Estimation of P(T|W)

P(T|W) is also, fundamentally, a time-to-event distribution and can again be estimated with PROC LIFETEST. The key point to remember is to exclude the appropriate patients based on the condition specified in W. Hence, when you compute P(T|W>9), you need to include only those patients with an SUV greater than 9, as shown in the following call to PROC LIFETEST:

```
proc lifetest data=lung(where=(suv>9)) timelist=(36);
    time survival*status(0);
run;
```

Output 7.2, which has the same format as Output 7.1 (with only the relevant portions shown), informs us that 1−S(T|W>9)=0.3665. The equation S(T|W≤9), obtained similarly (though not shown here), equals 0.8960.