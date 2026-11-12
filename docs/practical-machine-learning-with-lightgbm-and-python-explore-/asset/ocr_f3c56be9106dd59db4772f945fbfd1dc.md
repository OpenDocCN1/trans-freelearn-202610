A noteworthy part of SageMaker that simplifies one of the most important aspects of model development (bias and fairness) is **SageMaker Clarify**.

# ## SageMaker Clarify

Amazon SageMaker Clarify is a tool that provides greater transparency into ML models. SageMaker Clarify aims to assist in understanding how ML models make predictions, thereby enabling model explainability and fairness.

One of the primary features of SageMaker Clarify is its capacity to provide model interpretability. It helps developers understand the relationships between the input data and the model's predictions. The service generates feature attributions that show how each feature in the dataset influences predictions, which can be critical in many domains, especially those where it’s vital to understand the reasoning behind a model's prediction. In addition to providing insight into individual predictions, SageMaker Clarify offers global explanatory capabilities. It measures the importance of input features on a model's predictions in aggregate across the whole dataset. Feature impact analysis allows developers and data scientists to understand the overall behavior of a model, helping them interpret how different features drive model predictions on a global level.

Further, Clarify can help identify potential bias in trained models. The service includes pre-training and post-training bias metrics that help us understand if a model favors certain groups unfairly. It's best practice to check all new models for bias, but it is also imperative in regulated industries such as finance or healthcare, where biased predictions can have severe consequences.

Clarify provides model interpretability by using an advanced technique known as **SHapley Additive exPlanations (SHAP)**.

# ## SHAP

SHAP is a game theoretic approach to interpreting the output of any ML model [1]. SHAP aims to provide an understanding of the impact of individual features on a model's overall prediction.

Essentially, SHAP values assess the effect of a particular feature value by contrasting it with a baseline value for that feature, highlighting its contribution to the prediction. A SHAP value is a fair contribution allocation from each feature to the prediction for each instance. SHAP values are rooted in cooperative game theory, representing a solution to the following question:

> > “Given the difference a feature makes in predicting an outcome, what portion of that difference is attributable to each feature?”

These values are calculated using the concept of Shapley values from game theory. A Shapley value determines the significance of a feature by contrasting a model's predictions with the presence and absence of that feature. Yet, as the sequence in which a model encounters features can affect its prediction, Shapley values consider all possible orderings. Then, it assigns an importance value to a feature so that it equals the average marginal contribution of that feature across all possible coalitions.