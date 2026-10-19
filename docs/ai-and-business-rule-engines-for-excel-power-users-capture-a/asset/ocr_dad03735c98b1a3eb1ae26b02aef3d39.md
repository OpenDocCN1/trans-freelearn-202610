![](bbox=[0.246, 0.103, 0.773, 0.303])

Figure 11.20 – Referring to the ML model in our decision model

+   4. In our decision model, we dragged a new business model knowledge node—it’s the node on the very left of *Figure 11.18*. We edit it as in *Figure 11.21*:

![](bbox=[0.3, 0.405, 0.724, 0.609])

Figure 11.21 – The Business Model Knowledge node

There are several edits, including a name change, that we’ve made to the node. We set the type to **PMML**, hence the P in the top left of the screenshot. This prompts KIE Sandbox to ask us which document and model we want to refer to—`cr` and `chocolateRecommendation` values have been selected by dropdowns that will appear.

We’ve also edited the name and type of the parameters (Country_Code and Customer_Age)—the KIE sandbox has an editor to help guide you, but make sure that both match what is in the PMML model.

+   5. The actual call to the ML model is done via a decision node, an **Invocation** node. In the decision model we previously saw in *Figure 11.18*, it is called **Recommended Product**. Opening it in the KIE Sandbox editor should show you something like this: