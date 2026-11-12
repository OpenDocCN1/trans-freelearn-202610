![](bbox=[0.21, 0.08, 0.5, 0.14])

![](bbox=[0.155, 0.17, 0.89, 0.41])

Figure 3.8: Sorting by date

As you can see, sorting this field ordered the dates by the number of entries, thus making it very simple to answer the question, on which day were the most reviews entered? After the first few clicks, it already starts to feel natural, doesn't it?

All the changes you made to the dataset can be traced back on the left side of the profile pane. But don't forget to add a proper name to each step: this will make it much easier for others, and yourself, to understand your process.

So, let's continue on to the data quality features of Prep.

# # Data quality

Prep comes with lots of different features. Sometimes, you might use many different tools to prepare your dataset in order to get it in the shape you desire. Other times, you might just run an aggregation (one feature) and be done. It really depends on the dataset itself and the expected output. The fact is, the closer your Prep output data is to what you need for your Tableau Desktop visualization, the more efficiently VizQL will run on Tableau Desktop. Fewer queries in Tableau Desktop means faster generation of dashboards.

To me, the best part about Prep is that it can handle a huge amount of data. Sometimes, I even use it for datasets I don't want to visualize in Tableau Desktop, just to get a quick overview of, for example, how many rows contain a specific word, how many columns are needed, what happens to the date range if I filter a particular value, and so on! Within a few minutes, I have insights that would have taken me much more time to get with database queries or Excel functions. I hope that by the end of this chapter, you will be able to cut your time spent data prepping in half (at least). We will divide the prepping features into five subcategories: cleaning, unions and joins, aggregating, pivoting, and scripting. Let's start with cleaning data!