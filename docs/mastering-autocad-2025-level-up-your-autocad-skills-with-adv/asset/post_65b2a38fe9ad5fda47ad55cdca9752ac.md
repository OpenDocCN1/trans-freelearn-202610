
![](img/e5541e806f2ef46a4958b6c3d0b346a9_107_0.png)

Figure 4.12: The Units drafting setting with the flyout menu displayed and Decimal units selected

Using this as a drafting setting on the status bar is a much quicker and easier method to change the units in your drawing, rather than having to keep going to the **Drawing Units** dialog box.

### Tips and tricks #13

**Units** on the status bar, as a drafting setting, is for linear units only. You will still have to use the **Drawing Units** dialog box to alter any angular unit settings.

Units are only part of the design workflow in AutoCAD and allow you to draft effectively. You also need to communicate your design intent via annotation on your drawings. Next, we will look at using annotative scaling and annotation scales to enhance and improve your drawing legibility.

## Working with annotation scale to improve drawing legibility

The **Annotation Scale** feature (*CANNOSCALE*) is used when using annotation scaling in your drawings. It is often used for drawing elements such as dimensions, text, and hatching to make for a neat, easy-to-read drawing when working at differing viewport scales in the layout tabs.

![](img/e5541e806f2ef46a4958b6c3d0b346a9_109_0.png)

Figure 4.14: Dimensions with different annotation scales

In Figure 4.14, the larger dimensions (2500 and 2750) have an annotation scale of 1:50, and the smaller dimensions (250 and R1500) have an annotation scale of 1:30. These then correlate to the scale of the layout viewports in which they are displayed, by way of annotative scaling, using an annotation scale applied to each dimension object.

The benefit of these annotative dimensions is that due to the annotation scales applied to the dimension objects, they display consistently in the layout viewports. You can also apply multiple annotation scales so that annotative objects display in more than one scaled layout viewport.

All that is required for this to work effectively is an annotative dimension style. In the Office Project.dwg file, an annotative dimension style called Metric-mm is used. This is shown in Figure 4.15. The figure shows the Dimension panel, which you will find in the Annotate tab on the AutoCAD ribbon:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_109_1.png)

Figure 4.15: The current annotative dimension style shown in the Dimension panel

The annotative dimension style uses a small blue triangular symbol to indicate it is an annotative dimension style.

With an annotative dimension style, you set the annotation scale in the **Model** tab, using the flyout menu on the status bar, as shown in *Figure 4.16*:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_110_0.png)

Figure 4.16: The annotation scale flyout menu with 1:50 as the current annotation scale

Dimensions are added to your design in a drawing to communicate design intent. Typical examples of this would be the length and width of a room, the size of a window or door opening, or the shaft length and thread length of a bolt. You would normally add dimensions to a drawing using object snaps (OSNAP) to snap accurately to the geometry representing real-world objects to communicate these values for construction and manufacture. By setting the annotation scale when working in the **Model** tab, you are letting AutoCAD know how to scale the dimension in the appropriate layout tab.

Any subsequent dimensions placed using the annotative dimension style will adopt a 1:50 annotation scale, thus making sure they are displayed in the **1:50** layout viewport. The same workflow would be adopted for the **1:30** layout viewport, where the annotation scale for the annotative dimensions would be set to **1:30**.

All objects in an AutoCAD DWG file have known properties, whether that be the **Layer**, **Color**, **Linetype**, or in this case, the **Annotative Scale**. You can check whether a dimension object is annotative or not by looking at its properties in the **Properties** palette. The properties of an annotative dimension should be displayed as shown in *Figure 4.17*:

![](img/e5541e806f2ef46a4958b6c3d0b346a9_111_0.png)

Figure 4.17: The properties of a selected annotative dimension object displayed in the Properties palette

The biggest benefit of annotative scaling when used with annotative dimensions is that you can stick with one annotative dimension style and one layer for all your dimension annotation, knowing that it will display consistently in all layout viewports of differing scales.

### Tips and tricks #14

When using annotation scaling with annotative dimensions, you can also add multiple annotation scales to a single dimension object. For example, if you had a dimension that needed to be displayed in both a **1:50** layout viewport and a **1:30** layout viewport, you would add **1:50** and **1:30** annotation scales to the dimension object. This can be done via the **Properties** palette by clicking on the **Annotative scale** value (see Figure 4.17). After clicking on the value, a small button appears next to the displayed annotative scale value. Click on the button, and you will be able to add new annotation scale values using the dialog box that appears.

Annotation scaling is a fantastic time-saver. When using it with annotative dimensions, you only have to create one dimension style and one layer for your dimensions.

### AutoCAD for Mac

Annotation scaling is also available in AutoCAD for Mac. You would set up your annotative dimension styles using a similar workflow. Place your annotative dimension objects as you do in AutoCAD for Windows, using the annotation scale flyout menu on the status bar when working in the **Model** tab.

## Summary

In this chapter, we have learned how to select multiple overlaid objects and select the required object for updating or deletion, as well as how to model in 3D much more effectively using **3D Object Snap** (*3DOSNAP*).

We have also learned how to create accurate 2D isometric drawings, utilizing the isometric drafting (*ISODRAFT*) settings.

We have also gained valuable knowledge of the units (*UNITS*) function in AutoCAD, allowing for quick and effective changes to units in drawings, as well as learning how to set and/or change the annotation scale (*CANNOSCALE*) to work in line with annotative scales set in layout viewports in the drawing.

The advanced drafting settings in this chapter will allow you to work in a more effective way, enhancing your productivity. They also allow you to utilize various tools in AutoCAD to their full advantage. Now that you know how to use them, your daily AutoCAD use will become less onerous, and you will start to work smarter with AutoCAD, not harder.

In the next chapter, we will look at how you can develop advanced geometry in AutoCAD using object snaps.

## Technical requirements

To use AutoCAD, you’ll need to have an AutoCAD license installed either on your desktop computer or your laptop. You’ll also require the following:

- A subscription (license) to AutoCAD. This will need to be the *FULL* version of AutoCAD, not AutoCAD LT. Ideally, this will be the latest version of AutoCAD, which is *AutoCAD 2025* at the time of writing. You can use earlier versions, but it’s suggested that you go no further back than *AutoCAD 2022*.
- To use AutoCAD with a subscription, you’ll need an Autodesk account. If you don’t have one, you can set one up at https://accounts.autodesk.com/.
- This book covers AutoCAD 2025, running on the Microsoft Windows **operating system (OS)**. All images and graphics are from AutoCAD 2025 and the *Windows 11 OS*.

This book contains instructions for both Windows and Mac users. While comprehensive instructions have been provided for Windows users, special instructions relating to deviations in the Mac interface have also been provided within callout boxes.

## Exercise files

You’ll need to make sure you’ve downloaded the exercise file, `BearingCase.dwg`, to complete this chapter.

## Measuring angles accurately with Polar Tracking (POLAR)

Before working with **Polar Tracking** (*POLAR*), you’ll need to make sure it’s available on the AutoCAD status bar. Follow these steps to do so:

1. Click on the **Customization** (*hamburger*) icon and select **Polar Tracking**. A check mark will appear next to it.
2. Click on the **Customization** icon again to close the menu.
3. You’ll now see the **Polar Tracking** icon on the status bar. Ensure that it’s blue, indicating it’s been switched on.

*Figure 5.1 shows the **Polar Tracking** icon once it’s been switched on:*

![](img/e5541e806f2ef46a4958b6c3d0b346a9_116_0.png)

Figure 5.1: The AutoCAD status bar with Polar Tracking displayed and switched on
