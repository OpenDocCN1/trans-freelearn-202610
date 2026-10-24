# Data Exploration

# # Exploring numeric value distribution in a column

In this recipe, we will look at how to explore the properties of a column of numeric values. We will look at the tools that QGIS offers and apply them to analyze the elevation values in our sample POI dataset.

# ## Getting ready

To follow this recipe, please load `poi_names_wake.shp`. If you followed the previous recipe, *Listing unique values in a column*, you can continue directly from there.

# ## How to do it...

A good way to get a first impression of the properties of a numeric column is using the **Basic Statistics** tool from **Vector**. This allows you to calculate statistical values, such as the minimum and maximum values, mean and median, standard deviation, and sum.

If you want to examine the distribution of elevation values, there is the handy Statist plugin. Statist generates an interactive histogram representation of the value distribution:

+   1. Install **Statist** using **Plugin Manager**.
+   2. Start **Statist** from the **Vector** menu.
+   3. Specify **Input vector layer** and the attribute that you want to analyze (**Target field**), then click on **OK** to compute the statistics.
+   4. Using the buttons below the diagram, you can zoom and pan the diagram, as well as save the diagram image.
+   5. You can even customize the diagram by changing the title and axis labels and ranges. Just use the right-most button with the green tick mark on it to open the customization dialog: