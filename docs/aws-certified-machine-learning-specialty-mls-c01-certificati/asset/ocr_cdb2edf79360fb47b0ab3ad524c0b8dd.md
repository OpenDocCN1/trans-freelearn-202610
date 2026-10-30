# Dealing with outliers

We are not on this studying journey just to pass the AWS Machine Learning Specialty exam, but also to become better data scientists. There are many different ways to look at the outlier problem purely from a mathematical perspective; however, the datasets we use are derived from the underlying business process, so we must include a business perspective during an outlier analysis.

An **outlier** is an atypical data point in a set of data. For example, the following chart shows some data points that have been plotted in a two-dimension plan; that is, x and y. The red point is an outlier, since it is an atypical value on this series of data:

![](bbox=[0.11, 0.31, 0.88, 0.51])

Figure 3.19 – Identifying an outlier

We want to treat outlier values because some statistical methods are impacted by them. Still, in the preceding chart, we can see this behavior in action. On the left-hand side, we drew a line that best fits those data points, ignoring the red point. On the right-hand side, we also drew the best line to fit the data but included the red point.

We can visually conclude that, by ignoring the outlier point, we will come up with a better solution on the plan of the left-hand side of the preceding chart since it was able to pass closer to most of the values. We can also prove this by computing an associated error for each line (which we will discuss later in this book).

It is worth reminding you that you have also seen the outlier issue in action in another situation in this book: specifically, in Figure 3.17, when we had to deal with missing values. In that example, we used the median (instead of the mean) to work around the problem. Feel free to go back and read it again, but what should be very clear at this point is that median values are less impacted by outliers than average values.

You now know what outliers are and why you should treat them. You should always consider your business perspective while dealing with outliers, but there are mathematical methods to find them. Now, let’s look at these methods of outlier detection.