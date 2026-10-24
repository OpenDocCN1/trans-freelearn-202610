
# Calculating area shares within a region

Another spatial analysis task we often encounter is calculating area shares within a certain region, for example, landcover shares along one specific river. Using majrivers.shp and trees.shp, we can calculate the share of wooded area in a 10,000-foot-wide strip of land along the Susitna River:

- 1. We first define the analysis region by selecting the river and buffering it.

> QGIS Processing will only apply buffers to the selected features of the input layer. This default behavior can be changed under Processing | Options and configuration by disabling the Use only selected features option. For the following examples, please leave the option enabled.

To select the Susitna River, we use the Select by attribute tool. After running the tool, you should see that our river of interest is selected and highlighted.

- 2. Then we can use the Fixed distance buffer tool to get the area within 5,000 feet along the river. Note that the Dissolve result option should be enabled to ensure that the buffer result is one continuous polygon, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_124_0.png)

- 3. Next, we calculate the size of the strip of land around our river. This can be done using the Export/Add geometry columns tool, which adds the area and perimeter to the attribute table.
- 4. Then, we can calculate the Intersection between the area along the river and the wooded areas in trees.shp, as shown in the following screenshot. The result of this operation is a layer that contains only those wooded areas within the river buffer.

![](img/2e88bf5938bf48a409c0a534a8775b21_125_0.png)

- 5. Using the Dissolve tool, we can recombine all areas from the intersection results into one big polygon that represents the total wooded area around the river. Note how we use the Unique ID field VEGDESC to only combine areas with the same vegetation in order not to mix deciduous and mixed trees.

- 6. Finally, we can calculate the final share of wooded area using the **Advanced Python field calculator**. The formula `value = $geom.area() / <area>` divides the area of the final polygon (`$geom.area()`) by the value in the area attribute (`<area>`), which we created earlier by running **Export/Add geometry columns**. As shown in the following screenshot, this calculation results in a wood share of **0.31601** for **Deciduous** and **0.09666** for **Mixed Trees**. Therefore, we can conclude that in total, 41.27 percent of the land along the Susitna River is wooded:

![](img/2e88bf5938bf48a409c0a534a8775b21_126_0.png)

# Batch-processing multiple datasets

Sometimes, we want to run the same tool repeatedly but with slightly different settings. For this use case, Processing offers the Batch Processing functionality. Let's use this tool to extract some samples from our airports layer using the Random extract tool:

- 1. To access the batch processing functionality, right-click on the Random extract tool in the toolbox and select Execute as batch process. This will open the Batch Processing dialog.
- 2. Next, we configure the Input layer by clicking on the ... button and selecting Select from open layers, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_127_0.png)

- 3. This will open a small dialog in which we can select the airports layer and click on OK.
- 4. To automatically fill in the other rows with the same input layer, we can double-click on the table header of the corresponding column (which reads Input layer).
- 5. Next, we configure the Method by selecting the Percentage of selected features option and again double-clicking on the respective table header to auto-fill the remaining rows.
- 6. The next parameter controls the Number/percentage of selected features. For our exercise, we configure 10, 20, and 30 percent.
- 7. Last but not least, we need to configure the output files in the Extracted (random) column. Click on the ... button, which will open a file dialog. There, you can select the save location and filename (for example, extract) and click on Save.

- 8. This will open the Autofill settings dialog, which helps us to automatically create distinct filenames for each run. Using the Fill with parameter values mode with the Number/percentage of selected features parameter will automatically append our parameter values (10, 20, and 30, respectively) to the filename. This will result in extract10, extract20, and extract30, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_128_0.png)

- 9. Once everything is configured, click on the Run button and wait for all the batch instructions to be processed and the results to be loaded into the project.

# Automated geoprocessing with the graphical modeler

Using the graphical modeler, we can turn entire geoprocessing and analysis workflows into automated models. We can then use these models to run complex geoprocessing tasks that involve multiple different tools in one go. To create a model, we go to Processing | Graphical modeler to open the modeler, where we can select from different Inputs and Algorithms for our model.

Let's create a model that automates the creation of hexagonal heatmaps!

- 1. By double-clicking on the **Vector layer** entry in the **Inputs** list, we can add an input field for the point layer. It's a good idea to use descriptive parameter names (for example, `hex cell size` instead of just `size` for the parameter that controls the size of the hexagonal grid cells) so that we can recognize which input is first and which is later in the model. It is also useful to restrict the **Shape type** field wherever appropriate. In our example, we restrict the input to **Point** layers. This will enable Processing to pre-filter the available layers and present us only the layers of the correct type.

- 2. The second input that we need is a **Number** field to specify the desired hexagonal cell size, as shown in this screenshot:

- 3. After adding the inputs, we can now continue creating the model by assembling the algorithms. In the **Algorithms** section, we can use the filter at the top to narrow down our search for the correct algorithm. To add an algorithm to the model, we simply double-click on the entry in the list of algorithms. This opens the algorithm dialog, where we have to specify the inputs and further algorithm-specific parameters.

![](img/2e88bf5938bf48a409c0a534a8775b21_129_0.png)

- 4. In our example, we want to use the point vector layer as the **input** layer and the number input **hex cell size** as the **cells size** parameter. We can access the available inputs through the drop-down list, as shown in the following screenshot. Alternatively, it's possible to hardcode parameters such as the cell size by typing the desired value in the input field:

![](img/2e88bf5938bf48a409c0a534a8775b21_130_0.png)

> While adding the following algorithms, it is important to always choose the correct input layer based on the previous processing step. We can verify the workflow using the connections in the model diagram that the modeler draws automatically.

- 5. The final model will look like this:

![](img/2e88bf5938bf48a409c0a534a8775b21_131_0.png)

- 6. To finish the model, we need to enter a model name (for example, Create hexagonal heatmap) and a group name (for example, Learning QGIS). Processing will use the group name to organize all the models that we create into different toolbox groups. Once we have picked a name and group, we can save the model and then run it.

- 7. After closing the modeler, we can run the saved models from the toolbox like any other tool. It is even possible to use one model as a building block for another model.

Another useful feature is that we can specify a layer style that needs to be applied to the processing results automatically. This default style can be set using **Edit rendering styles for outputs** in the context menu of the created model in the toolbox, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_132_0.png)

# Documenting and sharing models

Models can easily be copied from one QGIS installation to another and shared with other users. To ensure the usability of the model, it is a good idea to write a short documentation. **Processing** provides a convenient **Help editor**; it can be accessed by clicking on the **Edit model help** button in the **Processing modeler**, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_132_1.png)
