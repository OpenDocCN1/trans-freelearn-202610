| Gender | F. Blood Sugar | W. Circum. | BMI | LDL Cholesterol | Prediction F 0(x) | Residuals for F 0(x) |
|---|---|---|---|---|---|---|
| Male | 105 | 110 | 29.3 | 170 | 125 | 45 |
| Female | 85 | 80 | 21 | 90 | 125 | -35 |
| Male | 95 | 93 | 26 | 113 | 125 | -12 |

# Table 2.4 – Based on our initial prediction, we can calculate the residuals for each patient, as shown in the Residuals for F 0(x) column

The next step is straightforward: we build a regression tree to predict the residuals. We do not use the predictions of the regression tree directly. Instead, we use the *terminal regions* to calculate our updated prediction. The terminal regions refer to the tree's leaf nodes.

For this example, we assume the following simple regression tree is built:

![](bbox=[0.322, 0.393, 0.673, 0.565])

# Figure 2.4 – Regression tree predicting the residuals

With our regression tree built and our leaf nodes defined, we can proceed with the next step. We need to compute $\gamma_{jm}$ that minimizes our loss function, taking into account the previous predictions as per $\gamma_{jm} = \text{argmin}_{\gamma} \sum_{x_i \in R_j} L(y_i, F_{m-1}(x_i) + \gamma)$. This is almost precisely what we did in *step 1*, where we showed that the equation simplifies taking the average of the predicted values due to our choice of the loss function. *Here, that means taking the average of the residuals in each leaf node.* Therefore, we have $\gamma_{1,1} = \frac{-35-12}{2} = -23.5$ and $\gamma_{2,1} = \frac{45}{1} = 45$.

Finally, we can now calculate our next prediction, $F_1(x)$, defined by: $F_m(x) = F_{m-1}(x) + v \sum_{j=1}^J \gamma_{jm} I(x \in R_{jm})$, meaning our next prediction consists of the previous prediction plus the $\gamma$ values calculated in *step 2.3*, weighed by a learning rate $v$. The summation here means that should a sample be part of multiple leaf nodes, we take the sum of the gamma values. Let's calculate $F_1(x)$ for the first sample in our dataset, using a learning rate of 0.1. According to the regression tree from *step 2.2*, our sample (which has a BMI > 26) maps to $\gamma_{2,1}$. Since it's only mapping to a single leaf, we don't need the summation part of the equation. Therefore, the equation looks like this: