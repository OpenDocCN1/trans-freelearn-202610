![](bbox=[0.08, 0.07, 0.87, 0.42])

Figure 11.21: Adding ID

Now, looking again at Figure 11.19, I selected Public health expenditure percentage of GDP 2013 on the X-Axis and Mean years of schooling - Years on the Y-Axis. I had to resize the X-Axis bin in the Data pane by right-clicking | Edit | Suggest Bin Size | OK. I can see that the overall correlation between these two measures is positively moderate at a correlation coefficient of 0.359. But by hovering over the dots, you can now investigate which countries have what effect on this correlation. For countries close to the trend line, the correlation coefficient is closer to the true factor. For example, the United States in the top-right corner has a value of 16.8 for the x-axis (health expenditure) and 12.76 for the y-axis (years of schooling). If you multiply 12.76 by 0.359 (correlation coefficient), you get 4.581, which is almost exactly the difference between 12.76 and 16.8. Now, if you search for Sierra Leone (bottom-right), you get x and y values of 11.4 and 2.9. Here, the multiplication with the correlation coefficient won't explain the relationship as well. This can be easily seen in the scatterplot because Sierra Leone is far away from the trend line. I really like this dashboard because it simplifies relationships between variables and can help novices as well as senior users understand data.

# Adding a correlation matrix

After clicking through the dashboard and testing the functionality, I realize that it's hard to remember which combinations I have already tried, and also, I wish I could filter for highly correlated combinations. To address this, I want to add one more dashboard to this workbook: a correlation matrix with all combinations, color-coded, and with an option to filter:

+   1. First, I go back to my Data Source tab and add the same dataset again, but with a new name: World Indices Correlation Matrix. I even add it twice because I need every index correlated against every other index: