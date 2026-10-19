Using a sample graph, let's draw a simple scatter plot in Excel, like the one shown in Figure 11.2. We've filtered the data a bit, to show the sales of just two product lines. We can identify two clear clusters—Americans (remember they have region codes around 100 in our data) tend to make large orders of Peanut Candy bars. And South Africans (region codes around 400) tend to make smaller orders of Lumpy Raisin and Nut bars.

In a very small nod to reality, we have one sale of Peanut Candy bar in South Africa—maybe it's somebody who has moved from New York to Johannesburg to study. That won't make difference to our analysis, but sometimes you may need to remove outliers such as this from the data:

![](bbox=[0.22, 0.29, 0.75, 0.51])

Figure 11.2 – Our sales data graphed as a scatter plot

You'll notice that in Figure 11.2, we've drawn a line across the chart to separate the two clusters. Given our simple chart, any order above the line is likely to be Peanut Candy, and anything below the line is likely to be a Lumpy chocolate bar. So, for new customers, we can make a recommendation based on their country code and order quantity based on this simple model.

This line can be drawn using the formula x=3y. Don't worry about the formula itself—remember that this is made-up data. What is important is that we can draw a line to separate those two groups. Once we learn what the formula is, we can use it to make predictions for new customers depending on which side of the line they fall on. That's what ML is—learning where to draw the line.

So far, our clusters have been easy to identify, but we had filtered our information down to two products. If we add Silk Tray sales to our graph, the situation is a lot more chaotic, as depicted in Figure 11.3: