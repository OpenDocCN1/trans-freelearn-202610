# # Missing values

Missing values denote the absence of a value for a variable. Since data can never be collected in a perfect manner, many missing values can appear due to human oversight, or can be introduced via any systematic process that touches a data element. It can be due to a survey respondent not completing a question, or, as we have seen, it can be created from joining a membership file with a transaction file. In this case, if a member did not have a purchase in a particular year, it might end up as NA or missing.

The first course of action for handling missing values is to understand why they are occurring. In the course of plotting missing values, you not only want to produce counts of missing values, but you want to determine which sub-segments may be responsible for the missing values.

To research this, attempt to break out your initial analysis by time periods and other attributes using some of the bivariate analysis techniques that have been mentioned. This will help you to identify where missing values may be tucked away.

# ## Setting up the missing values test dataset

We will start off by using two groups of generated data. One group is for males, who have a 3% probability of not responding to an age question in a survey, and the other group is for females, who have a 5% probability of not responding to an age question:

```
```r
library(wakefield)
library(dplyr)

#generate some data for Males with a 5% missing value for age

set.seed(10)
f.df <- r_data_frame(
 n = 1000,
 age,
```
```