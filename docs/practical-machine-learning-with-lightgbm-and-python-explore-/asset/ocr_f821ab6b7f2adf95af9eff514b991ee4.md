![](bbox=[0.193, 0.093, 0.488, 0.348])

![](bbox=[0.512, 0.093, 0.815, 0.348])

![](bbox=[0.193, 0.363, 0.488, 0.618])

![](bbox=[0.512, 0.363, 0.815, 0.618])

Figure 5.2 – A two-dimensional illustration of candidate solutions (red x marks) evolving with each subsequent generation to approximate the global optimum (located at the top and center of each landscape). In the context of CMA-ES, each candidate solution represents a combination of hyperparameter values, and the algorithm's performance determines the optimum

Central to the evolutionary process of CMA-ES is the covariance matrix. A covariance matrix is a square, symmetric matrix representing the covariance between pairs of variables (in the case of CMA-ES, the hyperparameters), providing insight into their relationships. The diagonal elements of the matrix represent the variances of individual variables, while the off-diagonal elements represent the covariances between pairs of variables. When there's a positive covariance, it signals that the variables usually move in the same direction, either increasing or decreasing. Conversely, a negative covariance points to a relationship where, as one variable rises, the other tends to fall, and vice versa. A covariance of zero suggests no linear relationship between the variables.