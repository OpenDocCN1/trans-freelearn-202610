# # Adding a simple node

A new node can be started by calling BeginNode(), using an int value as the unique identification for the node:

```
```cpp
const int nodeId = 1;
ImNodes::BeginNode(nodeId);
ImGui::Text("Sample Node");
ImNodes::EndNode();
```
```

Like in ImGui itself, all immnodes UI elements must have a unique ID set to be distinguishable by ImGui. Internally, immnodes uses the ImGui::PushID() call to set the ID for the group containing the node data. We only need to take care that all elements in the node editor have unique identifications set.

Inside the node, we can use all ImGui elements to create control elements. Some ImGui elements may behave unexpectedly. For instance, an ImGui::Separator() call draws a line up to the right end of the node editor. But the usual elements, like text, buttons, or sliders, are working as expected. Calling BeginDisable() and EndDisable() also works normally, allowing us to make parts of the node unavailable.

Be careful when disabling ImGui elements in a node

While it may seem tempting to completely remove unneeded ImGui elements from the node by enclosing them in if statements, be aware that immnodes calculates the position of input and output pins from the IDs given to the attributes. If you want to remove attributes or ImGui elements, make sure to keep the same IDs for the attributes and elements remaining visible, or else the links will be attached at the wrong places.

Figure 9.4 shows the result of the previously found lines of code:

![](bbox=[0.35, 0.85, 0.65, 0.93])

Figure 9.4: The sample node