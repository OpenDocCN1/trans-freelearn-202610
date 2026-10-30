
3. You are working for a call center company, and the business team wants to see the percentage of calls on each channel of their interactive voice response device. You just got those percentages, broken down per month. What’s the best way to show this monthly information to your users?

+   a) Pie chart
+   b) Stacked 100% column chart
+   c) Histogram
+   d) Scatter plot

Answer
B, In this case, you want to show the composition of your data across different periods. Both a and b could be used to show compositions; however, only option b would be appropriate to show those compositions across each month.

4. You are working as a data scientist for a telecom company. The company offers many different services related to broadband, TV, phone, and mobile. In total, the company offers more than 50 services. You want to see the number of services that each customer usually signs for. What’s the most appropriate chart to use to prepare for this visualization?

+   a) Pie chart
+   b) Stacked 100% column chart
+   c) Histogram
+   d) Scatter plot

Answer
C, Since the company has so many services and you want to see the distribution of the number of services per customer, the most appropriate chart would be a histogram. Keep in mind that, most of the time, when the goal of the analysis is showing distributions, your most common options are histograms and box plots. Depending on the use case, scatter plots can also be used.

5. You are working on a sales report for your company and you need to make different types of comparisons regarding your company's sales across different regions. Which of the following types of charts are suitable for presenting comparisons in your data (choose all the correct answers)?

+   a) Bar chart
+   b) Column chart
+   c) Line chart
+   d) Histogram

Answer
A, B, C, You have multiple options when it comes to using charts to present comparisons on your data. Use bar charts when you have a single variable and no time dimension; use column charts when you have one or two variables changing over time; and use line charts when you have three or more variables changing over time. Finally, remember that it is also possible to show comparisons using tables. This is especially helpful when you have three or more variables.

6. You are working as a data scientist for a meteorological organization. Your company is measuring the daily temperature of a particular region during the entire summer. The summer is over and it is now time to present a detailed report about the data that has been captured. Which of the following options are not valid types of analysis (choose all correct answers)?

+   a) Create a box plot to show some statistics about the data; for example, the median temperature, lower and upper quartiles, and outlier points.
+   b) Create a histogram to show the distribution of the data across all the different days when temperatures have been measured.
+   c) Create a key performance indicator just to present the average temperature that has been measured during the summer, in that particular region.
+   d) Create a line chart to show the evolution of the temperature, day after day. That will give you some sense about the increasing and decreasing trends.

# Answer

All the answers are valid types of analysis for this particular problem. Box plots and histograms would be very useful to present the distribution of the data; a KPI would provide a straight answer to the question "What was the average temperature?"; and line charts could be used to show the trends in the data during the period of measurement. There are many other types of analysis you could use for this problem. Just as another example, you could think of using a column chart to show the average temperature every month.

7. You are working in a financial company and your team is responsible for the investment portfolio. You have multiple options for investments and the key characteristics of all of them are annual rate, investment period, and investment amount. There is a straight relationship between these three components. You have built a bubble chart to show the relationship among these investment options, but now, you want to compare them and add a few more dimensions, such as level of risk and year after year performance. What is the most appropriate way to present this information in the most compact way?

+   a) Change the current bubble chart that you created previously, by adding these new dimensions.
+   b) Create a table to present this information, where each investment option is represented by rows and each metric is represented by columns.
+   c) Keep the current bubble chart as-is and create a few more visualizations to present the new metrics.
+   d) Keep the current bubble chart as-is and present the rest of the information in a tabular structure.

# Answer

B, You should be aware that, during your exam, you might find questions with multiple correct answers; however, there will be an optimal one. In this case, only one option would be wrong, but all the rest are quite possible. Option b is the most accurate answer since the question is asking about the "most compact way" the present the information. Pay attention to specific keywords in the questions during your exam.

8. You are working as a data scientist for a marketing company. At the moment, you are building a machine learning model to predict churn. One of the features that you are using on your model is annual salary. Due to the nature of this variable, you suspect that a log transformation would be beneficial to bring this feature closer to a normal distribution. Which of the following charts would support you to prove that, indeed, a log transformation is a good idea for this feature?

+   a) Histogram
+   b) Scatter plot
+   c) Line chart
+   d) Stacked bar chart

Answer
A, Deciding on whether log transformation is a good idea or not will depend on the distribution of the data. If you have a skewed distribution, then log transformation could be beneficial to bringing this data closer to a normal distribution. Depending on the algorithm you are using for modeling, such a transformation can improve the accuracy of your model. The histogram is the right answer to this question since it will present the data distribution that will support your decision.

9. You are working as a data analyst for a tech company. Your company wants to analyze sales across different lines of services, and you've decided to use Quick Sight as a data visualization tool. Which of the following options is false about Quick Sight?

+   a) Quick Sight imports datasets into SPICE to optimize data processing. With SPICE, your analytical queries process quickly and you don't need to wait for a direct query process.
+   b) Although Quick Sight can encrypt data transfers, it does not support encryption at rest.
+   c) Quick Sight can connect on Redshift, Aurora, Athena, RDS, on-premises databases, S3, and SaaS.
+   d) When access is granted to Active Directory groups, users from that group won't be notified.

**Answer**
B, All these answers are correct, except option b. Quick Sight does support encryption at rest, but only in the enterprise edition. Another important feature that's only supported in the enterprise edition is integration with AD.

10. You work in a company that uses Quick Sight for data visualization purposes. Which of the following types of analysis are supported by Quick Sight (choose all the correct answers)?

+   a) Analyzing relationships in the data
+   b) Analyzing comparisons in the data
+   c) Analyzing compositions in the data
+   d) Analyzing data distributions

**Answer**
Quick Sight is a great tool that supports all the use cases that we have learned about in this chapter. It not only supports these use cases but also optimizes data processing through the use of SPICE.

## 5

## AWS Services for Data Storing

AWS provides a wide range of services to store your data safely and securely. There are various storage options available on AWS such as block storage, file storage, and object storage. It is expensive to manage on-premises data storage due to the higher investment in hardware, admin overheads, and managing system upgrades. With the AWS Storage services, you just pay for what you use, and you don’t have to manage the hardware. We will also learn about various storage classes offered by Amazon S3 for intelligent access of the data and reducing costs. You can expect questions in the exam on storage classes. As we continue in this chapter, we will master the single-AZ and multi-AZ instances, and RTO (Recovery Time Objective) and RPO (Recovery Point Objective) concepts of Amazon RDS.

In this chapter, we will learn about storing our data securely for further analytics purposes by means of the following sections:

- Storing data on Amazon S3
- Controlling access on S3 buckets and objects
- Protecting data on Amazon S3
- Securing S3 objects at rest and in transit
- Using other types of data stores
