
Though we are dealing with structured tabular data, some columns contain pure text data, such as the destination or name feature. While it may seem obvious to drop these features, it might be possible to apply data transformation and extract useful information that can help make better predictions. When working with these features, the FEATURE DETAILS section of the report can tokenize the features that contain text and display useful information such as word frequency that can help us to make feature transformation decisions.

The FEATURE DETAILS section is an important place to determine what specific feature transformation steps we will add to our data wrangling process.

## Step 3 – creating data analysis

As we covered in the preceding section, the built-in data analysis report that is available out of the box with Data Wrangler covers a lot of commonly used EDA in machine learning; however, you can also create additional analysis and custom analysis in Data Wrangler. In this section, we will cover some of these:

- **Target leakage** is a common issue in machine learning, where information from the target variable or outcome is used to create features or predictors, resulting in overly optimistic or biased models. Target leakage occurs when a feature that is created using information that is not available during the time of prediction or decision-making is used to train a predictive model. Target leakage analysis is crucial because it helps ensure that predictive models are built on valid and unbiased data. In Data Wrangler, you can generate a target leakage analysis by clicking on the + icon next to the datasets and choosing Add Analysis. In the Analysis Type dropdown, choose Target Leakage. Then, choose the problem type and an option from the Target dropdown.

  ![](img/e28602b44ec813a47df57c96b204412c_360_0.png)

  *Figure 10.18: Target leakage*

  As you can see, in our dataset, the boat feature has a likely target leakage issue. The boat feature contains the lifeboat number if the passenger was on one. It is possible that this feature is not available for all the records of our dataset and was populated post-prediction. We will look at how to handle this in the data transformation and feature engineering step.

- Scatter plot analysis: Scatter plots represent the relationship between two or three variables. In our example dataset, we will use the scatter plot to visualize the relationship between fare, age, and the target variable, survived. You can add a new analysis in the same way as you added the previous analysis. In the Analysis type dropdown, choose Scatter Plot, and then choose age as the X axis and fare as the Y axis. To add the target as another variable, choose survived under the Color by dropdown.

  ![](img/e28602b44ec813a47df57c96b204412c_361_0.png)

  While there may not be a clear trend or relationship between age and fare, we can observe some patterns in the scatter plot that provide additional insights into the data. For example, we can find that passengers who paid higher fares had a higher survival rate. Alternatively, we can find that children had a higher survival rate compared to adults, showing a potential age-related factor in survival.

- Custom visualizations: Besides using the pre-defined analysis and visualizations, you can also create your own custom data visualizations in Data Wrangler using Altair. Altair is a Python data visualization library that allows users to easily create interactive visualizations with concise and intuitive code. It also integrates well with other Python data analysis libraries, such as pandas, NumPy, and SciPy, making it a popular choice for data scientists and analysts. Data Wrangler uses the df variable to store the dataframe, which is accessible by the user. You can add a custom visualization similar to the preceding by clicking on the + button of any step and choosing Add analysis. Set the analysis type as Custom Visualization. Here, we will create a dataset.

The **variance inflation factor (VIF)** is a measure of collinearity among variables. We calculate it by solving a regression problem to predict one variable when given the rest. A VIF score is a positive number that is greater than or equal to 1, and a score of 1 means the variable is completely independent of the others. The larger the score, the more dependent it is. As a rule of thumb, we can drop or feature-engineer features with a VIF score of greater than 1.

To create multicollinearity analysis with VIF, choose **Add Analysis** and then **Multicollinearity**.

![](img/e28602b44ec813a47df57c96b204412c_363_0.png)

*Figure 10.21: Multicollinearity using VIF*

Once you click on the preview, you will see a histogram that has the feature names on the *X* axis and the VIF score on the *Y* axis. In our example, we can see that we do not have any features that have multicollinearity based on VIF.

- Besides VIF, there are a couple of other multicollinearity analyses that you can choose out of the box with Data Wrangler. Let's look at **Lasso feature selection analysis**. The Lasso method involves adding a penalty term to the regression model that shrinks the coefficients of the independent variables toward zero. This penalty term can help to identify which independent variables are most important to predict the dependent variable and can also help to address issues of multicollinearity. To use the Lasso feature selection method in Data Wrangler, choose the **Multicollinearity** analysis as discussed previously and then set **Analysis** as **Lasso feature selection**. You can optionally choose the L1 magnitude, which is the penalty term; the default value is **1**. Next, we set the problem type as **Classification** and the label column as **Survived**. This will create a baseline regression model, making all the independent variables in the dataset where the regularization parameter is set to be sufficiently large, causing some of the coefficients to be shrunk toward zero. This will create a histogram with the most important features – that is, features that have a non-zero coefficient.

![](img/e28602b44ec813a47df57c96b204412c_364_0.png)

*Figure 10.22: Multicollinearity using Lasso feature selection*

Check the correlations between the selected independent variables to determine whether there are any issues of multicollinearity. By using the Lasso feature selection method in this way, you can identify and address multicollinearity in a regression model, which can help to improve its accuracy and reliability.

Besides the analysis that we covered here, Data Wrangler has the capability to analyze other specific types of datasets, such as time-series data. These let you do an additional analysis for anomaly detection and detecting seasonal trends. Now that you have a good understanding of your data and the underlying patterns, you can move on to the next step of modifying your data by adding transformations.

## Step 4 – adding transformations

As part of your data analysis, you might have noticed elements of your dataset that you want to change or transform. The goal of data transformation is to make data more suitable for modeling, to improve the performance of machine learning algorithms, or to handle missing or corrupted values. Data transformations for machine learning can include things such as normalization, standardization, data encoding, and binning. Not all datasets are alike, and not all transformations apply to all datasets. The goal of data analysis is to identify specific transformations for your dataset. While we typically apply data transformation as an early step in the machine learning pipeline, before data is used to train a model, in real-world machine learning, we continually monitor our model performance and apply transformations as necessary. After you have imported and inspected your dataset in Data Wrangler, you can start adding transformations to your data flow. You may have noticed that some columns in our dataset do not have the correct data type associated with them. For example, the age feature in our dataset has a String type instead of Numeric. Before we apply transformations, we recommend reviewing and fixing the data types for all the features in our dataset.

You can review the data type that Data Wrangler automatically assigned to your features.
