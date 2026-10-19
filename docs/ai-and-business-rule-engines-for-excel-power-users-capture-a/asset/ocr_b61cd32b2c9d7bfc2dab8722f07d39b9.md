Based on what we can see in the diagram, KIE now knows we want to feed a name into the greeting decision. But we still haven’t told KIE how it’s going to make its decision – let’s fix that now. Click on the rectangular box **greeting** node, as shown in *Figure 3.8*:

![](bbox=[0.417, 0.18, 0.6, 0.271])

Figure 3.8 – Editing node properties

Then, from the icons that appear, click on the **Edit** icon (the square with the pencil, on the middle left). The screen will change to something like the screenshot in *Figure 3.9*:

![](bbox=[0.389, 0.372, 0.62, 0.628])

Figure 3.9 – Editing the Decision node type

You may need to click on the **Select expression** box to view this drop-down menu to display the different logic types available. Select **Literal expression**.

> > **A quirk of the editor**
> >
> > Be careful – in the current version of KIE Sandbox, there is no way to change the logic type once you have chosen it.
> >
> > If you do choose the wrong type of expression, you’ll need to go back to the main decision diagram, delete the entire greeting node, and then create an entirely new one (including renaming the node and connecting the InputData node to it).