To represent a value dimension on this specific location on the map, we most often use a color palette and color the data points according to a predefined color range. However, you should be aware that using color palettes to display numeric values is somehow ambiguous because we perceive color changes differently from size changes. In addition, the designer can control the perception of the user by picking a certain color palette. In the next figure, for example, we see three different visualizations that all display the same data with different color palettes. Whereas, the left and right visualizations look very similar and give the user a similar understanding of the data, the visualization in the middle gives a completely different and contradictory image of the data. This is achieved by simply inverting the color palette from the left visualization. Also, the quantization of the color steps can have a huge impact. In one example, we could generate a very smooth gradient; but with a different quantization, we could create visible borders between the quantizations:

![](bbox=[0.1, 0.315, 0.95, 0.49])

Displaying the same data using different color palettes

It's up to the designer to choose a color palette that goes along with the data that visualizes the aspects one wants the user to see and understand.

# Can we start now?

Now, you will have a good overview of which parts and components are needed to draw cartographic visualization. In the following chapters, we will go into details and implement some visualizations using D3.

The only part we have not discussed until now is where we can get the data for geographic visualization and in which format the data is encoded. We will discuss this right ahead in the following section.