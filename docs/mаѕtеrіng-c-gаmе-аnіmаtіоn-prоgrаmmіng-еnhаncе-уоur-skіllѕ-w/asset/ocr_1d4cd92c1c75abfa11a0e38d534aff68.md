For the user interface, we use the names from the bone name list of the instance’s model. Since we have a 1:1 relationship between the bone IDs and the names, iterating over the list to create a combo box is easy.

Together with a Boolean variable to enable or disable inverse kinematics and an iteration slider, the user interface for the section about inverse kinematics for the model’s feet looks like Figure 12.12:

![](bbox=[0.21, 0.23, 0.79, 0.36])

Figure 12.12: Configured inverse kinematics for the Women model

For Figure 12.12, the Women model was used as an example. For other models, the node names may be different.

Inverse kinematics depends on the model

The ability to configure inverse kinematics for the feet of a model depends heavily on the model’s skeleton. It’s likely that some models you find on the internet may miss some parent-child relationships between nodes of the skeleton, for instance between leg and feet nodes, leading to non-working inverse kinematics.

The FABRIK algorithm has another part that adjusts the world positions of the original nodes by using the calculated positions. We skipped the code for the final part in the Implementing the FABRIK inverse kinematics algorithm section since we have to split the FABRIK algorithm for technical reasons – the node data used by the FABRIK algorithm resides on the GPU, but some of the calculations are easier to handle on the CPU.

Let’s combine the node positions now.

# Adjusting the node positions

Due to the usage of compute shaders, the matrices containing the rotation, translation, and scale plus the data for the world positions of the nodes only reside in GPU memory. Plus, the node positions are calculated for performance reasons in a compute shader. We need to copy the data