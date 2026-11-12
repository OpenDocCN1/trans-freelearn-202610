A variable that is not MCAR and not MAR can be considered **Not Missing at Random (NMAR)**. That basically means that the missing value is related to the values of the missing value itself, or is related to a variable that's not in the model. NMAR values often show up in survey research and clinical research where subjects are measured over time. For example, a survey respondent who reported severe depression at the beginning of a study may drop out at the end of the study, so if they scored high on a depression scale, that would be related to the fact they have missing scores at a later date, since they might be more inclined to drop out.

NMAR is the most difficult situation to detect, since a variable may seem to be MAR or MCAR and really be NMAR. Missing value analysis is not an exact method, and again, that is where understanding the domain, and the underlying methods of generating data, is so important.

# Correcting for missing values

Although it is always important to understand the source of your missing values, how you ultimately handle them depends upon the technique that you use to analyse your data sets. For example, classification methods such as decision trees and random forests know how to deal with missing values, since they can treat them as a separate class, and you can safely leave them in the model. However, if a variable has a large amount of missing values, say > 20%, you might want to look at imputation techniques, or try to find a better variable that measures the same thing.

# Listwise deletion

For MCAR, eliminating rows that have missing values is acceptable. In this example (from [https://en.wikipedia.org/wiki/Listwise_deletion](https://en.wikipedia.org/wiki/Listwise_deletion)), observations 3, 4, and 8 will be removed from the data prior to analysis: