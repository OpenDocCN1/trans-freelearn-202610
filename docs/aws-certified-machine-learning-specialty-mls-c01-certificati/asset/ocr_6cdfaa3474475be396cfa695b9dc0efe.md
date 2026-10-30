In Chapter 1, Machine Learning Fundamentals, you were introduced to the feature classification shown in the preceding diagram. Now, let's look at some real examples in order to eliminate any remaining questions you may have:

| Feature type | Feature sub-type | Definition | Example |
|---|---|---|---|
| Categorical | Nominal | Labeled variables with no quantitative value | Cloud provider: AWS, MS, Google |
| Categorical | Ordinal | Adds the sense of order to a labeled variable | Job title: Jr Data Scientist, Sr Data Scientist, Chief Data Scientist |
| Categorical | Binary | A variable with only two allowed values | Fraud classification: Fraud, Not Fraud |
| Numerical | Discrete | Individual and countable items | Number of students: 1000 |
| Numerical | Continuous | Infinite number of possible measurements and they often carry decimal points | Total amount: $150.35 |

Figure 3.2 – Real examples of feature values

Although looking at the values of the variable may help you find its type, you should never rely only on this aspect. The nature of the variable is also very important for making such decisions. For example, someone could encode the cloud provider variable (shown in the preceding table) as follows: 1 (AWS), 2 (MS), 3 (Google). In that case, the variable is still a nominal feature, even if it is now represented by discrete numbers.

If you are building a ML model and you don't tell your algorithm that this variable is not a discrete number, but instead a nominal variable, the algorithm will treat it as a number and the model won't be interpretable anymore.

> **Important note**
> Before feeding any ML algorithm with data, make sure your feature types have been properly identified.