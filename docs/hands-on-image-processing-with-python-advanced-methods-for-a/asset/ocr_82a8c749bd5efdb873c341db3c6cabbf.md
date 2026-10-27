The predictive mean is $\bar{p} = \frac{1}{N} \sum_{i=1}^{N} p_i$ and uncertainty is estimated from the sample variance:

$\sigma^2 = \frac{1}{N} \sum_{i=1}^{N} (p_i - \bar{p})^2$

This produces uncertainty estimates without explicitly learning weight distributions.

**Results:** The example output:

```
Deterministic NN Accuracy: 0.9722
MC Dropout Accuracy:       0.9750
```

The next visualization displays the predicted digit and uncertainty value $\sigma$ for each test image, this time with MC dropout.

![](bbox=[0.22, 0.35, 0.75, 0.48])

Unlike the *full Bayesian network*, *MC Dropout* often achieves accuracy comparable to deterministic models while still providing useful uncertainty estimates, as can be seen in the following table.

| Method | Learns Weight Distributions | Uncertainty Estimate | Computational Cost |
|---|---|---|---|
| Deterministic NN | No | No | Low |
| Bayesian NN | Yes | Excellent | High |
| MC Dropout | Approximate | Good | Moderate |

*Table 12.5: Deterministic vs. Bayesian Network for image classification*

The *Bayesian neural network* provides the most principled treatment of uncertainty by learning *probability distributions over weights*, whereas *MC Dropout* approximates *Bayesian inference* through *stochastic dropout sampling*. Although deterministic networks often achieve slightly higher classification accuracy, *Bayesian* approaches provide valuable *confidence estimates* that can reveal ambiguous, noisy, or out-of-distribution inputs.

In summary, Bayesian neural networks transform image classification from predicting a single answer to predicting a distribution of plausible answers, enabling uncertainty-aware decision making. MC Dropout offers a practical approximation that retains many Bayesian benefits while requiring only minor modifications to a standard neural network.