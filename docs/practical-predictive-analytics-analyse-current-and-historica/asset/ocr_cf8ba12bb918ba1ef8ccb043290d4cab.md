# # Outliers

Outliers are values in the data that are outside the range of what is to be expected. "What is to be expected?" is of course subjective. Some people will define an outlier as anything beyond three standard deviations of a normal distribution, or anything beyond 1.5 times the interquartile ranges. This, of course, may be good starting points, but there are many examples of real data that defies any statistical explanation. These rules of thumb are also highly dependent upon the form of the data. What might be considered an outlier for a normal distribution would not hold for a lognormal or Poisson distribution.

In addition to potential single variable outliers, outliers can also exist in multivariate form, and are more prevalent as data is examined more closely in a high-dimensional space.

Whenever they appear, outliers should be examined closely since they may be simple errors or provide valuable insight. Again, it is best to consult with other collaborators when you suspect deviation from the norm.

# ## Why outliers are important

Outlier detection is important for a couple of reasons. First, it allows you to learn a lot about the extremes in your data. Typical data is usually easy to explain. If you have many values of a certain category, it is usually easy to track down an explanation. It is the extreme values that can add additional insight beyond the typical, or identify faulty processes which can be fixed.

Additionally, outliers have a profound effect upon some algorithms. In particular, regression methods can be biased by the presence of outliers and lose power because of them.

# ## Detecting outliers