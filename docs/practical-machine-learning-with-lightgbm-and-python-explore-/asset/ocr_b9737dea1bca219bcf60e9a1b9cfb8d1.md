| Algorithm | Accuracy | F1 score |
|---|---|---|
| Decision tree | 59.87% | 0.57 |
| Random forest | 69.35% | 0.67 |
| LightGBM | 70.00% | 0.68 |

Table 6.4 – Five-fold cross-validated performance metrics on the Credit Score dataset

As shown in Table 6.4, the LightGBM model performs the best, slightly outperforming the random forest model accuracy. Both algorithms perform better than the decision tree baseline. The LightGBM model also trained the fastest, more than seven times faster than the random forest model.

We now proceed with parameter optimization of the LightGBM model.

# Model tuning

Similar to the previous case study, we use Optuna for parameter optimization. We again use the TPE sampler with an optimization budget of 50 trials.

# Model deployment and results

Our model is now prepared and ready to be deployed to a platform of our choice. Potential deployment options would include building a web API around our model, deploying with a tool such as PostgresML, or using a cloud platform such as AWS SageMaker. These and other options are discussed in detail in the coming chapters.

We could also use the model as part of a data science report. See the previous section for details on writing a good report. Keep in mind the most important aspects of communicating data science results, as follows:

+   • Always report results in an unbiased and fair way
+   • Keep your audience in mind and focus on the report's details that provide them with the most value

# Summary

This chapter presented two case studies on how to apply the data science process with LightGBM. The data science life cycle and the typical constituent steps were discussed in detail.

A case study involving wind turbine power generation was presented as an example of approaching a data problem while working through the life cycle. Feature engineering and how to handle outliers were discussed in detail. An example exploratory data analysis was performed with samples given for visualization. Model training and tuning were shown alongside a basic script for exporting and using the model as a program.