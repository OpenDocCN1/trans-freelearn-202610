We have now reached the end of this long – but very important – chapter about data preparation and transformation. Let's take this opportunity to do a quick recap of the awesome things we have learned.

# Summary

First, you were introduced to the different types of features that you might have to work with. Identifying the type of variable you'll be working with is very important for defining the types of transformations and techniques that can be applied to each case.

Then, we learned how to deal with categorical features. We saw that, sometimes, categorical variables do have an order (such as the ordinal ones), while other times, they don't (such as the nominal ones). You learned that one-hot encoding (or dummy variables) is probably the most common type of transformation for nominal features; however, depending on the number of unique categories, after applying one-hot encoding, your data might suffer from sparsity issues. Regarding ordinal features, you shouldn't create dummy variables on top of them, since you would be losing the information of order that's been incorporated into the variable. In those cases, ordinal encoding is the most appropriate transformation.

We continued our journey by looking at numerical features, where we learned how to deal with continuous and discrete data. We walked through the most important types of transformations, such as normalization, standardization, binning, and discretization. You saw that some types of transformation rely on the underlying data to find their parameters, so it is very important to avoid using the testing set to learn anything from the data (it must be used strictly for testing).

You have also seen that we can even apply pure math to transform our data; for example, you learned that power transformations can be used to reduce the skewness of your feature and make it more normal.

Next, we looked at missing data and got a sense for how critical this task is. When you are modeling, you can't look at the missing values as a simple computational problem, where you just have to replace x with y. This is a much bigger problem where you need to start solving it by exploring your data, and then checking if your missing data was generated at random or not.

When you are making the decision to remove or replace missing data, you must be aware that you are either losing information or adding bias to the data, respectively. Remember to review all the important notes we gave you, since they are likely to be, somehow, present in your exam.