# How to fix these errors

Go to our Decision Model and select the name input node, as shown on the left-hand side of *Figure 3.19*:

![](bbox=[0.132, 0.172, 0.849, 0.524])

Figure 3.19 — Changing the name data type to string

In this figure, we’ve already clicked on the *Properties* icon at the top right of the screen to bring up the **Properties** panel on the right-hand side.

We then set Data type to **string** (bottom right of the figure). *String* is just another name for a piece of text.

As soon as we make this change, one of the errors should disappear from the **Error** panel, since we told KIE to expect this node to supply strings as an input type.

It’s a similar process for the *greeting* node (right-click the node, edit properties, and set Data type to **string**). The second warning error should be cleared since we told KIE that this decision node takes text as its input type.