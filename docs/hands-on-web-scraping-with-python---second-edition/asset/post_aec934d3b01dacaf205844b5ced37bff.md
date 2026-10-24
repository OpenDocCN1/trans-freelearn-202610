
## Regression

Regression estimates the relationship between variables. It is also known as a **predictive algorithm**. A regression model attempts to determine the strength of the relationship between dependent variables and one or more independent variables. The output variable in regression is generally a quantitative value. Normally, regression techniques are applied to make predictions from a given set of inputs called **predictors**. A strong statistical background is necessary to handle regression algorithms.

Listed here are a few common types of regression algorithms:

- **Linear regression:** Also known as **Simple Linear Regression (SLR)**, this is used to predict one (dependent variable) value based on the value of one independent variable. It finds the linear correlation between variables, using the best-fitting straight line. It is best for small datasets and results in continuous output.
- **Multilinear regression:** Also known as **Multiple Linear Regression (MLR)** is a type of linear regression model. It models linear and non-linear relationships with more than one independent variable and one dependent variable. The output reflects the influence or relationship between multiple independent and single dependent variables.
- **Logistic regression:** This is also known as a *statistical regression* model or probabilistic algorithm, which helps in analyzing datasets. This algorithm is used to find the best-fitting model that calculates the probability of binary types (such as yes and no). It also describes the relationship between binary (the involvement of two possible outcomes) dependent variables and sets of predictors. In terms of the number of variables, the algorithm describes the relationship between multiple independent and single dependent variables. This regression technique results in a discrete output and is used mostly with classification-related problems.
- **Polynomial regression:** Polynomial regression describes *non-linear relationships*. Generally, if there’s no possibility of a linear relationship between dependent and independent variables, it might be a case of polynomial regression. Polynomial regression is also considered a special case of MLR. With a trained model, a non-linear equation is created and used for prediction.

There are many more forms of regression learning. Measuring the effectiveness of algorithms is also quite challenging. A few techniques, such as **Ordinary Least Squares (OLS)** and **R-Squared (R2)**, are used to minimize the possibility of errors. OLS estimates the coefficients of linear regression. It minimizes the sum of squared residuals between actual and predicted values. R2, with values between 0 and 1, calculates the variation percentage of the dependent variable. The greater the value of R2, the better the model is.

Regression-based learning can be used in cases such as weather forecasting, sale price prediction, house price prediction, and stock markets.

## Important note

There are a few supervised ML algorithms that can be used for both regression and classification. Logistic regression is most often used for classification-related problems. SVM, being a deep learning model, is used for both classification and regression. Similarly, algorithms such as random forest, kNN, and decision tree are also used for both types of supervised learning.

Choosing the best learning model depends on the type of data, the volume of data, the correlation between columns of data, processing time, expected output, the inclusion of variation in data, and many more factors.

With data collected using web scraping, a supervised form of ML is the most used format for analysis and decision-making. In the next section, we will explore unsupervised ML.

## Unsupervised learning

Unsupervised learning is effective in discovering trends and finding hidden patterns in data. In unsupervised learning, the algorithms are left to detect, explore, and learn on their own. There is nothing to supervise the learning process. This type of learning is beneficial in the field of data mining, and also when we are not sure of what we are looking for.

In comparison to supervised learning, unsupervised learning tasks are complex, with many challenges. In unsupervised learning, time to prepare data and perform preliminary analysis is not required. Hidden or unknown patterns of data are exposed, which is not possible in supervised learning. Results from unsupervised learning might be low in accuracy, in comparison to supervised learning.

Unsupervised learning is mostly divided into two parts:

- Clustering
- Association

In the next section, types of unsupervised learning are introduced.

## Clustering

Clustering is a process of grouping, sorting, categorizing, and classifying data or objects. Classification models in supervised learning also group and categorize input objects. Classification models are provided with predefined categories or group names, whereas in clustering the groups or categories are generated after processing the data.

Groups or clusters of data generated will contain data with similar characteristics and types. The flexibility of clustering is the most in-demand feature. Learning models, once trained, can be used in various other scenarios.

**K-means clustering**, one of the most widely used clustering algorithms, demands the value of k. Here, k refers to the number of clusters that are to be created or the data that is to be divided and put into groups. It identifies a seed point as a centroid (mean point) in the current cluster and collects data in a cluster that is nearest to the centroid.

Similar to kNN, distance measurement (*Euclidean*) is conducted between pieces of data or data points, and is used to create or separate clusters that are similar and dissimilar. kNN and k-means clustering are often categorized as search-based (learning) algorithms. The data points use themselves to find other data points.

## Association

Association helps to discover new patterns, find relationships in data, and produce information from large datasets. It identifies the frequency of patterns in a dataset. In addition, it maps the data based on its dependencies.

Association, also known as **Association Rule Mining (ARM)**, tries to identify associations among dataset variables. One of the common algorithms used for ARM is **Apriori**, which discovers the most frequent data items in datasets and identifies associations and correlations among them. ARM is normally processed in large datasets, but is likely to carry lots of noise as there is the possibility of the number of associations being infinite. Most of the time, binary attributes such as yes and no are used to define association rules.

Please refer to *Chapter 10* for more detailed information. In the next section, we will learn about the basics of the reinforcement type of learning.

## Reinforcement learning

This type of learning algorithm uses and recognizes its environment and learns the behaviors by accessing data patterns or trains its learning model based on trial and error (also known as the reward and punishment model). Reinforcement learning is about making decisions or learning actions (based on the situation) and their feedback, which results in the maximum reward.

Reinforcement learning algorithms learn from a chain of action that is almost similar to decision tree processing. For example, situation-based input results in output that becomes input for the next cycle and so on, until a decision is made or obtained.

**Reinforcement Agents (RAs)**, also called AI-driven systems, are self-trained and make the decision to resolve an assigned task based on the training data. To analyze or recognize situations, external agents and systems such as software (logs), system settings, browser-based extensions, cameras in machines, and more are used to find the most information possible about the situation.

So far in this chapter, information regarding ML-related features has been explored. Now, we will build an ML model and use practical concepts of some ML types that can be applied for predictions and analyzing sentiments.
