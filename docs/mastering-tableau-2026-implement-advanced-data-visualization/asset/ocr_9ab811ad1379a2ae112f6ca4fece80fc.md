# # Cleaning data

We have seen the following canvas before in the The Prep GUI section. To create the cleaning step, the user can simply click on + next to the input and select Add: Clean Step. During the cleaning step, multiple operations can be performed, such as filtering or creating a calculated field. Also note the recommendations Prep gives you:

![](bbox=[0.07, 0.17, 0.84, 0.42])

Figure 3.9: Recommendations

Prep analyzes the column content and proposes changes that might fit the data. The column listing_url, for example, is being recognized as a web page and therefore Prep recommends you change it to the data role URL. The second, third, and several more recommendations after listing_url are to remove certain columns. This is probably the case because the column does not contain any data or contains only a small amount of data. The list goes on.

This feature can be very useful, especially for unfamiliar datasets. My way of working would be to look at the recommendations, check whether they make sense, and execute the change—or not. Don’t blindly trust these recommendations, but they can point out data flaws you might have missed otherwise.

Data is often messy, involving null values, typos from manual entries, different formatting, changes in another system, and so on. As a consequence, you will have to sort out the mess before you can get reliable results from an analysis or a dashboard. We will look at how to clean data at the column level.

Once a value is selected within your clean step, you have the option to Keep Only, Exclude, Edit Value, or Replace with Null:

![](bbox=[0.13, 0.755, 0.82, 0.795])

Figure 3.10: Quick access