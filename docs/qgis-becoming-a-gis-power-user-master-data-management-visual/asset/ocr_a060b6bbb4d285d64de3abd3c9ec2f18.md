Let's create a model that automates the creation of hexagonal heatmaps!

+   1. By double-clicking on the **Vector layer** entry in the **Inputs** list, we can add an input field for the point layer. It's a good idea to use descriptive parameter names (for example, `hex cell size` instead of just `size` for the parameter that controls the size of the hexagonal grid cells) so that we can recognize which input is first and which is later in the model. It is also useful to restrict the **Shape type** field wherever appropriate. In our example, we restrict the input to **Point** layers. This will enable Processing to pre-filter the available layers and present us only the layers of the correct type.
+   
+   2. The second input that we need is a **Number** field to specify the desired hexagonal cell size, as shown in this screenshot:
+   
+   3. After adding the inputs, we can now continue creating the model by assembling the algorithms. In the **Algorithms** section, we can use the filter at the top to narrow down our search for the correct algorithm. To add an algorithm to the model, we simply double-click on the entry in the list of algorithms. This opens the algorithm dialog, where we have to specify the inputs and further algorithm-specific parameters.

![](bbox=[0.177, 0.349, 0.823, 0.614])