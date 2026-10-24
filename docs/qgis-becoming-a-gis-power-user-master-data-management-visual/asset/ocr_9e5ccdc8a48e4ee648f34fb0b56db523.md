# Classic Vector Analysis

+   - Buffer the roads using **Fixed distance buffer** (refer to the **Buffer roads** step). The buffer size is controlled by the number model input called **road_buffer_size**. You can extend this approach of controlling the model parameters using additional inputs to all the other buffer steps in this model. (We chose to show only one example in order to keep the model screenshot readable.)
+   - Use **Intersection** to get areas near schools (refer to the **Intersection: near schools** step).
+   - Use **Intersection** to get areas near schools and lakes (refer to the **Intersection: schools and lakes** step).
+   - Use **Difference** to remove areas near roads (refer to the **Difference: avoid roads** step).

This is how the final model looks like:

![](bbox=[0.175, 0.369, 0.823, 0.599])

You can run this model from the **Processing Toolbox** option, or you can even use it as a building block in other models. It is worth noting that this model produces intermediate results in the form of buffer results (near_elementary, near_highschool, and so on). While these intermediate results are useful when developing and debugging the model, you may eventually want to remove them. This can be done by editing the buffer steps and removing the **Buffer <OutputVector>** names.