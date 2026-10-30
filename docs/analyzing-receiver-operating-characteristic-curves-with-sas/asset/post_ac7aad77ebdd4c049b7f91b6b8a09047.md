
benefit an ROC-focused discussion. Think of neural networks as a specific yet flexible class of nonlinear regression models.

Before we run decision trees and neural networks on the same data set, there is one important issue to consider about missing data. Both logistic regression and neural networks require complete observations. If any portion of a record is missing, that record is completely ignored during model building. Decision trees are more forgiving. They consider surrogate splits in case the variable for the primary split is missing. For this reason, if we run logistic regression, neural networks, and decision trees on the same input data, the actual data that the predictive model rests on may not be identical from one method to the other. You could address this by imputing the value from a distribution derived from the normal density. An imputation node can be dragged and dropped from the Modify tab, as seen in Figure 9.4. The same figure also shows the addition of decision tree and neural network nodes, both dragged and dropped from the Model tab.

## Figure 9.4 SAS Enterprise Miner Diagram Workspace for the Competing Models Example

![](img/ac26020abd4f22abfaafc0607939c6e4_125_0.png)

The flow in Figure 9.4 implies that the same data set will be analyzed three different ways and the results compared in the final node.

Three ROC curves, one for each method of analysis, are plotted to better assess the predictive power of the models (see Figure 9.5). Neural networks and logistic regression have very similar performance. The decision tree outperforms regression and neural networks. It has higher sensitivity than the other two for a given value of specificity for the most part, especially in the range where both sensitivity and specificity are above 0.5. The neural network method is slightly better than the regression method, although the difference between the two seems to dissipate in the validation and test sets. Output 9.2 reports the AUCs for each model and data set. The AUC for the decision tree is consistently above 85%, while the other two models’ AUCs cluster around 80%.

## Output 9.2

|  | Train: | Valid: | Test: |
| Model | Roc | Roc | Roc |
| Node | Index | Index | Index |
|  |  |  |  |
| Neural | 0.84003 | 0.78812 | 0.80917 |
| Reg | 0.81356 | 0.78147 | 0.79139 |
| Tree | 0.86871 | 0.85724 | 0.85971 |

## Figure 9.5 ROC Curves from SAS Enterprise Miner for Each of the Three Models and Data Sets

![](img/ac26020abd4f22abfaafc0607939c6e4_126_0.png)

## 9.5 ROC Curves Using PROC GPLOT with Exported Data from SAS Enterprise Miner

While SAS Enterprise Miner provides a built-in capability for plotting the ROC curve, sometimes you might want to output the data set containing the ROC points so that you can use the %PLOTROC macro for custom graphics. This can be done by selecting View→Table, which displays the SAS data set behind the ROC curve. At this point, selecting File→Save As gives you the option to save the table as a permanent data set that can be accessed later using Base SAS and SAS/GRAPH.

To give you some ideas, Output 9.3 lists the first 23 observations in the data set produced by SAS Enterprise Miner using the flow in Figure 9.4. The key variables are SENSITIVITY and ONEMINUSSPECIFICITY. Additional useful variables are MODELNODE and DATAROLE. The MODEL variable contains four categories: _Baseline_ (denoting the model where no covariate information is used, which results in the 45-degree line), Neural (corresponding to the Neural Network node), Reg (corresponding to the Regression node), and Tree (corresponding to the Decision Tree node). DATAROLE simply indicates whether the corresponding observation is for training, validation, or testing only. Traditionally, ROC curves are plotted only for the validation data set, but you can compare the amount of overestimation with the availability of this variable in Output 9.3.

## Output 9.3

| Obs | Node | Model Node | Variable | Role | Event | Specificity | Sensitivity |
|---|---|---|---|---|---|---|---|
| 1 | Neural | _Baseline_ | BAD | TRAIN | 1 | 0.00000 | 0.00000 |
| 2 | Neural | _Baseline_ | BAD | TRAIN | 1 | 1.00000 | 1.00000 |
| 3 | Neural | _Baseline_ | BAD | VALIDATE | 1 | 0.00000 | 0.00000 |
| 4 | Neural | _Baseline_ | BAD | VALIDATE | 1 | 1.00000 | 1.00000 |
| 5 | Neural | _Baseline_ | BAD | TEST | 1 | 0.00000 | 0.00000 |
| 6 | Neural | _Baseline_ | BAD | TEST | 1 | 1.00000 | 1.00000 |
| 7 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.08000 |
| 8 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.09263 |
| 9 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.10737 |
| 10 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.11368 |
| 11 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.12000 |
| 12 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.12421 |
| 13 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.12632 |
| 14 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.13263 |
| 15 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.13474 |
| 16 | Neural | Neural | BAD | TRAIN | 1 | 0.00000 | 0.14737 |
| 17 | Neural | Neural | BAD | TRAIN | 1 | 0.00052 | 0.15579 |
| 18 | Neural | Neural | BAD | TRAIN | 1 | 0.00052 | 0.16632 |
| 19 | Neural | Neural | BAD | TRAIN | 1 | 0.00052 | 0.18316 |
| 20 | Neural | Neural | BAD | TRAIN | 1 | 0.00210 | 0.19579 |
| 21 | Neural | Neural | BAD | TRAIN | 1 | 0.00315 | 0.20000 |
| 22 | Neural | Neural | BAD | TRAIN | 1 | 0.00315 | 0.20842 |
| 23 | Neural | Neural | BAD | TRAIN | 1 | 0.00367 | 0.21263 |

| Obs | True Positive | True Negative | False Positive | False Negative | First In Group | Last In Group | Up Posterior | Low Posterior |
|---|---|---|---|---|---|---|---|---|
| 1 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 2 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 3 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 4 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 5 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 6 | 357 | 0 | 1433 | 0 | 0.01638 | 0.01414 | 0.01649 | 0.00000 |
| 7 | 38 | 1907 | 0 | 437 | 0.99957 | 0.99066 | 1.00000 | 0.98994 |
| 8 | 44 | 1907 | 0 | 431 | 0.98922 | 0.98331 | 0.98994 | 0.98054 |
| 9 | 51 | 1907 | 0 | 424 | 0.97777 | 0.96834 | 0.98054 | 0.96766 |
| 10 | 54 | 1907 | 0 | 421 | 0.96698 | 0.96111 | 0.96766 | 0.95897 |
| 11 | 57 | 1907 | 0 | 418 | 0.95682 | 0.94955 | 0.95897 | 0.94757 |
| 12 | 59 | 1907 | 0 | 416 | 0.94559 | 0.93612 | 0.94757 | 0.93349 |
| 13 | 60 | 1907 | 0 | 415 | 0.93087 | 0.93087 | 0.93349 | 0.92554 |
| 14 | 63 | 1907 | 0 | 412 | 0.92020 | 0.91320 | 0.92554 | 0.91079 |
| 15 | 64 | 1907 | 0 | 411 | 0.90838 | 0.90838 | 0.91079 | 0.90047 |
| 16 | 70 | 1907 | 0 | 405 | 0.89257 | 0.88287 | 0.90047 | 0.88271 |
| 17 | 74 | 1906 | 1 | 401 | 0.88255 | 0.87657 | 0.88271 | 0.87213 |
| 18 | 79 | 1906 | 1 | 396 | 0.86769 | 0.86015 | 0.87213 | 0.85448 |
| 19 | 87 | 1906 | 1 | 388 | 0.84880 | 0.84035 | 0.85448 | 0.83941 |
| 20 | 93 | 1903 | 4 | 382 | 0.83848 | 0.83108 | 0.83941 | 0.82600 |
| 21 | 95 | 1901 | 6 | 380 | 0.82092 | 0.81388 | 0.82600 | 0.81205 |
| 22 | 99 | 1901 | 6 | 376 | 0.81022 | 0.80096 | 0.81205 | 0.79862 |
| 23 | 101 | 1900 | 7 | 374 | 0.79628 | 0.78792 | 0.79862 | 0.78480 |

The ROC curves in Figure 9.6 are identical to the ones in the second panel of Figure 9.5 and are generated by PROC GPLOT using the following statements. You can exercise more control on the aesthetic and functional aspects of the curve with PROC GPLOT.

```
axis1 length=12cm order=0 to 1 by 0.2 label=(f=swissb h=2)
value=(font=swissb h=2);
axis2 length=12cm order=0 to 1 by 0.2 label=(a=90 f=swissb h=2)
value=(font=swissb h=2);

symbol1 v=none i=join w=2 c=black l=33 h=1.5;
symbol2 v=none i=join w=2 c=black l=1 h=1.5;
symbol3 v=none i=join w=2 c=black l=3 h=1.5;
symbol4 v=none i=join w=2 c=black l=9 h=1.5;

legend1 position=(inside right bottom) frame across=1;

proc gplot data=em(where=(datarole='VALIDATE'));
  plot sensitivity*oneminusspecificity=model / haxis=axis1
vaxis=axis2 legend=legend1;
run;
quit;
```

## Figure 9.6 ROC Curves for the Validation Data Using PROC GPLOT

![](img/ac26020abd4f22abfaafc0607939c6e4_128_0.png)
