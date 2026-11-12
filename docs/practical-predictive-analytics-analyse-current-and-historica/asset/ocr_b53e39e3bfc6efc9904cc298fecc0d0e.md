# Simulating the data

Once we have calculated the mean values and covariance matrices for all of the columns, we are ready to simulate a big dataset for any number of observations we desire.

# Which correlations to use?

For the covariance matrix, we can either use separate matrices for the two diabetes outcomes (1,0), or use a pooled covariance matrix, which shows the correlations among the variables regardless of the outcome.

We will use the separate correlation or covariance matrices since we have enough observations for each outcome (n=500 and n=268). If either of these classes were much smaller related to the other, we could use the pooled (or total) covariance matrix instead, since that would cover a larger set of observations.Â

Some notes on the code which follows:

+   - As a reminder, always start with a random seed prior to a simulation. That will ensure that you get the same random results every time you run the code.

+   - The `cor()` function will compute the correlation matrix among all of the variables, while the `cov()` function computes the covariance matrix. The major difference between a correlation and covariance matrix is that covariance is expressed in the same units as the variables, while correlation standardizes the relationship to a value between 0 and 1. However, they essentially measure the same thing.

+   - The `nbins` variable will divide the data into individual bins with 400 observations each. This will be handy to extract smaller samples of the data rather than performing analysis on the entire dataset. Â Again, this is another way of demonstrating that sampling is your friend.

+   - The datasets are generated using the `mvrnorm` function, which takes an