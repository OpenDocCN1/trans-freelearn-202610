+   • Extrapolation, as you might infer, is the model's ability to predict values outside of the range defined by our training data. A typical example of extrapolation is forecasting – that is, predicting the future. In our previous example, if we ask the model to make a prediction in December, we expect it to extrapolate from the training data.

Of the two types of generalization, extrapolation is much more challenging and may require a specific type of model to achieve. However, in both cases, a model can overfit the data, losing its ability to interpolate or extrapolate accurately.

**Overfitting** is a phenomenon where the model fits the training data too closely and loses its ability to generalize to unseen data. Instead of learning the underlying pattern in the data, the model has memorized the training data. More technically, the model fits the noise contained in the training data. The term noise stems from the concept of data containing signal and noise. Signal refers to the underlying pattern or information captured in the data we are trying to predict. In contrast, noise refers to random or irrelevant variations of data points that mask the signal.

For example, consider a dataset where we try to predict the rainfall for specific locations. The signal in the data would be the general trend of rainfall: rainfall increases in the winter or summer, or vice versa for other locations. The noise would be the slight variations in rainfall measurement for each month and location in our dataset.

The following graph illustrates the phenomenon of overfitting:

![](bbox=[0.23, 0.496, 0.76, 0.818])

Figure 1.2 – Graph showing overfitting. The model has overfitted and predicted the training data perfectly but has lost the ability to generalize to the actual signal