There are many different statistical modeling techniques you can choose for churn analysis, including regression, decision trees, random forests, naive bayes, and neural networks.

Survival analysis is a good technique to use with customer churn problems since it is able to deal specifically with two aspects of marketing data, which other techniques have problems with. These involve the concepts of time-dependent data and censoring.

# Time-dependent data

In many analytic techniques, all data is treated as static at the time of analysis, and these techniques are not well equipped to deal with data that changes over time. Any event that measures a change in a variable over time, such as age, or changes in attitudes via surveys can be treated as time-dependent variables, and are handled well by survival techniques. This works well within a marketing concept since the treatment of different customer segments can change, and we need a mechanism to measure the effect of interventions such as promotions and advertising that can change customer behavior over time.

# Censoring

In general, censoring is a term that is used to describe data that is only partially known. The reason it is considered partially known is that all data is constrained between the starting and ending periods of an observational study. This can be information that occurs before a study has begun or after a study has ended. If all the information is contained within the study period itself, the data is not censored. However, this rarely happens.

Censored data can be left or right censored.

# Left censoring

In a marketing context, studies usually begin with customers already in place, and without any knowledge of how they were acquired.