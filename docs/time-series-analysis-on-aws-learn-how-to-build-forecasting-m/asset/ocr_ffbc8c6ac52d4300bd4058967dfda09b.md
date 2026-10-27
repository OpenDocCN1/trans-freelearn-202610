The vertical dotted line (located around 2% in the preceding screenshot) materializes the value that each signal importance would have if they were all equally contributing to every anomaly detected. The dataset illustrated here had 50 signals. If every signal was contributing the same way, they would each be ranked at approximately 1/50 = 2.0%. We can now say that the higher contributions from the top 5 signals are definitely statistically significant.

Every signal higher than the black dotted line will have a red bar, whereas the other ones will have a green bar.

# Signal importance evolution

The last widget plots the evolution of the feature importance of each signal over time. When numerous events are detected, this plot can become very crowded. At this stage, the feature importance is aggregated at the daily level to help better understand the dynamics of each signal, as shown in the following screenshot:

![](bbox=[0.155, 0.411, 0.849, 0.761])

Figure 12.12 – Signal importance evolution example