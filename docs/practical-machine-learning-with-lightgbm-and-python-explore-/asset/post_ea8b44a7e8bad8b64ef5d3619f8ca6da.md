
## Pareto Front Plot

![](img/31b70e4a77c3afbe11032f68ab874b02_120_0.png)

Figure 5.5 – A scatter plot showing the Pareto front for a MOO study

As shown in Figure 5.5, the F1 score is poor if the learning rate is too low and picks up quickly as the learning rate gets to 0.01. The F1 score peaks at 0.12 and slowly trails off as the learning rate increases. We now have the necessary information to decide on our trade-off: we can choose a higher learning rate for faster training, sacrificing the minimum amount of classification performance.

### Summary

This chapter introduced Optuna as a framework for HPO. We discussed the problems of finding optimal hyperparameters and how HPO algorithms may be used to find suitable parameters efficiently.

We discussed two optimization algorithms available in Optuna: TPE and CMA-ES. Both algorithms allow a user to set a specific budget for optimization (the number of trials to perform) and proceed to find suitable parameters within the constraints. Further, we discussed the pruning of unpromising optimization trials to save additional resources and time. Median pruning and the more complex but effective pruning techniques of successive halving and Hyperband were discussed.

We then proceeded to show how to perform HPO studies for LightGBM in a practical example. We also showed advanced features of Optuna that can be used to save and resume studies, understand the effects of parameters, and perform MOO.

The next chapter focuses on two case studies using LightGBM, where the data science process is discussed and applied in detail.

### References

- [1] J. Bergstra, R. Bardenet, Y. Bengio, and B. Kégl, “Algorithms for Hyper-Parameter Optimization,” in Advances in Neural Information Processing Systems, 2011.
- [2] J. Bergstra, D. Yamins, and D. Cox, “Making a Science of Model Search: Hyperparameter Optimization in Hundreds of Dimensions for Vision Architectures,” in Proceedings of the 30th International Conference on Machine Learning, Atlanta, 2013.
- [3] N. Hansen and A. Ostermeier, “Adapting arbitrary normal mutation distributions in evolution strategies: the covariance matrix adaptation,” in Proceedings of IEEE International Conference on Evolutionary Computation, 1996.
- [4] K. Jamieson and A. Talwalkar, Non-stochastic Best Arm Identification and Hyperparameter Optimization, 2015.
- [5] L. Li, K. Jamieson, G. DeSalvo, A. Rostamizadeh, and A. Talwalkar, Hyperband: A Novel Bandit-Based Approach to Hyperparameter Optimization, 2018.

## Solving Real-World Data Science Problems with LightGBM

With the preceding chapters, we have slowly been building out a toolset for us to be able to solve machine learning problems. We've seen examples of examining our data, addressing data issues, and creating models. This chapter formally defines and applies the data science process to two case studies.

The chapter gives a detailed overview of the data science life cycle and all the steps it encompasses. The concepts of problem definition, data exploration, data cleaning, modeling, and reporting are discussed in a regression and classification problem context. We also look at preparing data for modeling and building optimized LightGBM models using our learned techniques. Finally, we look deeper at utilizing a trained model as an introduction to **machine learning operations (MLOps)**.

The main topics of this chapter are as follows:

- *The data science life cycle*
- *Predicting wind turbine power generation with LightGBM*
- *Classifying individual credit scores with LightGBM*

### Technical requirements

The chapter includes examples and code excerpts illustrating how to perform parameter optimization studies for LightGBM using Optuna. Complete examples and instructions for setting up a suitable environment for this chapter are available at https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-6.

### The data science life cycle

Data science has emerged as a critical discipline, enabling organizations to derive valuable insights from their data and drive better decision-making. At the heart of data science lies the data science life cycle, a systematic, iterative process that guides data-driven problem-solving across various industries and domains. This life cycle outlines a series of steps that data scientists follow to ensure they address the right problem and deliver actionable insights that create real-world impact.

The first stage of the data science life cycle involves defining the problem, which entails understanding the business context, articulating objectives, and formulating hypotheses. This crucial stage establishes the entire project's foundation by establishing a clear direction and scope. Subsequent stages in the life cycle focus on data collection, preparation, and exploration, collectively involving gathering relevant data, cleaning and preprocessing it, and conducting exploratory data analysis to unveil patterns and trends.

Once the data is analyzed, the data science life cycle progresses to model selection, training, evaluation, and tuning. These stages are central to developing accurate and reliable predictive or descriptive models by choosing the most appropriate algorithms, training them on the preprocessed data, and optimizing their performance. The goal is to build a robust model that generalizes well to unseen data and addresses the problem at hand effectively.

Lastly, the data science life cycle emphasizes the importance of deploying the final model into a production environment, monitoring its performance, and maintaining it to ensure its ongoing relevance and accuracy. Equally important is the communication of results and insights to stakeholders, which is vital for driving informed decision-making and realizing the full potential of data science. By following the data science life cycle, organizations can systematically extract value from their data and unlock new opportunities for growth and innovation.

With previous examples, we followed a loose recipe of steps for working with data and creating models. In the next section, we formally define and discuss the steps of the data science life cycle.

### Defining the data science life cycle

The following are the key steps broadly applied in the data science life cycle. The steps are also shown in Figure 6.1, which illustrates the cyclical nature of the life cycle.

![](img/31b70e4a77c3afbe11032f68ab874b02_125_0.png)

Figure 6.1 – Diagram depicting the data science life cycle

These are the key steps:

- 1. **Define the problem:** Clearly articulate the business problem, goals, and objectives. This stage involves understanding stakeholder requirements, formulating hypotheses, and determining the project’s scope. Defining the problem also sets the stage for data collection and can determine how we’ll utilize our models.

- 2. **Data collection:** Gather the required data from various sources, such as databases, APIs, web scraping, or third-party data providers. Ensure the data is representative, accurate, and relevant to the problem. It is important to document where data originates and how it is moved around to establish the data lineage. Further, build a data dictionary that documents the data’s format, structure, content, and meaning. Importantly, validate any potential bias in the collection or sampling of data.

- 3. **Data preparation**: Clean and preprocess the data to make it suitable for analysis. This stage includes tasks such as **data cleansing** (e.g., handling missing values and removing duplicates), **data transformation** (e.g., normalization and encoding categorical variables), and **feature engineering** (e.g., creating new variables or aggregating existing ones). Moving and joining the data to where it can be analyzed and modeled might also be necessary.

- 4. **Data exploration**: Conduct exploratory data analysis (EDA) to gain insights into the data. This step involves visualizing data distributions, identifying trends and patterns, detecting outliers and anomalies, and checking for relationships and correlations between features.

- 5. **Model selection**: Choose the most appropriate data modeling techniques based on the problem type (e.g., regression, classification, or clustering) and the data characteristics. It's important to choose multiple model algorithms to validate performance on the data set.

- 6. **Model training**: Train the selected models using the prepared data. This step involves splitting the data into training and validation sets, setting model parameters (hyperparameters), and fitting the models to the data.

- 7. **Model evaluation**: Assess the performance of the trained models using appropriate evaluation metrics (e.g., accuracy, precision, recall, F1-score, **Area under the ROC Curve (AUC-ROC)**, or **root mean square error (RMSE)**) and compare them to select the best-performing model(s). Perform cross-validation or use holdout test sets to ensure an unbiased evaluation.

- 8. **Model tuning**: Fine-tune the selected model by optimizing hyperparameters, feature selection, or incorporating domain knowledge. This step aims to improve the model's performance and generalization to unseen data. It might also be appropriate to tune the model for the specific problem; for instance, when recognizing faces, a higher precision is more appropriate than a high recall.

- 9. **Model deployment**: If the model is to be part of a more extensive software system, deploy the final model into a production environment, where it can be used to make predictions or inform decision-making. Deployment may involve integrating the model into existing systems, creating APIs, or setting up monitoring and maintenance procedures.

- 10. **Model monitoring and maintenance**: Continuously monitor the model's performance and update it as necessary to ensure it remains accurate and relevant. Techniques such as detecting model and data drift should be used to ensure model performance. Model maintenance may involve retraining the model with new data, updating features, or refining the problem definition.

- 11. **Communicate results**: Share insights and results with stakeholders, including any recommendations or actions based on the analysis. Communicating the results may involve creating visualizations, dashboards, or reports to communicate the findings effectively.

We now examine two case studies to see how the data science life cycle is applied practically to real-world data. We look at a regression problem, predicting wind turbine power generation, and a classification problem, classifying individual credit scores.
