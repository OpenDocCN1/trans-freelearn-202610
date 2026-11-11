# What Just Happened?

You just used the attribution control with two layers containing an `attribution` property.
The text on the bottom right of the map shows the current active layers' attribution text.
Let's continue going over more of the built in control classes.

# OpenLayers.Control.EditingToolbar

The **EditingToolbar** control is a panel composed of tools that allow drawing and editing of vector features. It required a vector layer to be present in your map. We will cover this control, and how to use it, in much detail in Chapter 9.

# OpenLayers.Control.Graticule

This will display lines of longitude and latitude (or whichever measurements your projection is) on your map. Unlike some of the other controls, this control has quite a few properties that we can set to change the way it behaves. This is what our map looks like after adding the Graticule control with the default parameters:

![](bbox=[0.3, 0.43, 0.7, 0.73])

Let's go over the properties of the **Graticule** control.