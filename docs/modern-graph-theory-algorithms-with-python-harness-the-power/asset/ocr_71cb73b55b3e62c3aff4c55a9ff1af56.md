# # Introduction to temporal data

In *Chapter 2*, we briefly introduced temporal data or data in the form of a time series or a group of time series. **Time series data** tracks important metrics in many different industries: daily store sales volumes, weekly software product marketing lead volumes, daily incidence of an emerging disease, yearly behavior rates (such as smoking or vegetable consumption) in a population, or hourly stock prices tracking market trends. Many related factors can influence trends over time, and some models consider these factors directly if they are known in advance.

However, consider the case of sales trends for a new gem store in a city where gem stores are a new phenomenon, perhaps somewhere rural between Haifa and Tel Aviv (*Figure 6.1*). Thus, there is very little known about what might influence sales. Understanding what trends exist in the time series data is critical when mining for factors that might influence sales. However, time series datasets pose significant challenges to many supervised learning methods, such as **random forest models or linear regression**. At one point in time, sales are not independent; they rely on factors that influence sales in the previous days, limiting the use of supervised learning and many types of **unsupervised learning**. The lack of predictors also poses a challenge.

![](bbox=[0.275, 0.42, 0.725, 0.786])

Figure 6.1 – An illustration of a gem store located partway between Tel Aviv and Haifa, Israel