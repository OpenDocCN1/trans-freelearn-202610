# # Solving industry-specific analysis problems

We will touch on a few industries to discuss special consideration for IoT data exploration and analysis.

# ## Manufacturing

For IoT data generated during the manufacturing process, the accuracy of recorded values is especially important. Explore the data for outliers and analyze distributions carefully. Verify all the data ranges and distributions that you see with the experts on the manufacturing process.

The benefits of making sure the measurement values are clean as possible are two fold. First, any machine learning models created to detect problems will be significantly more accurate. Secondly, false positives due to invalid data can have a high penalty. The manufacturing line and product deliveries may be halted while the issue is investigated. In manufacturing, this can get expensive quickly. More perniciously, the long-term effect of false positives tends to be the complete rejection of the analytics by company management, when they no longer trust the numbers. This can cause even more expense to your company in the long run.

For IoT data generated from the product after it is delivered and in operation, the age of the product is key. Many issues are tied to how long the product has been in use, whether it is early in life (infant mortality) or late in life (wear out) problems. Age will need to be tracked carefully and investigated closely in your exploratory analytics.

Something else to watch out for: many inexperienced analysts calculate problem rates by grouping populations by production periods and comparing them by dividing the number of failures by the number of units built. Failures are typically determined by an IoT device communicating a fault code or by an abnormality in a measured value. There is a problem with this though, and it is a big one.

Since units built earlier have had more time in operation, they have more opportunity to experience a failure. Newer built units have had less opportunity. Units will continue to age, and the average time in operation for a production period will only grow over time. They are at uneven points in their operational life, as the datasets are not complete.