![](bbox=[0.14, 0.09, 0.21, 0.13])

*a small space (from 10 to 15 pixels) from the top of the band to the labels. This will give your report a cleaner layout, adding a small distance between the title bar and the details header.*

To complete the task, select the `Actuals` field and the `Budget` field from the Details band and click on the right alignment icon (≡) in the font toolbar. At the end of the task, the Report canvas should look like the following image:

![](bbox=[0.12, 0.24, 0.79, 0.4])

![](bbox=[0.14, 0.47, 0.21, 0.52])

*With the default setup, the details header will be shown in the first pag repeat the header on each page, select the `Details Header` item from the s tab in the Report explorer. In the `Style` tab of the Element properties, yo the `repeat-header` property in the `page-behaviour` group. Change the value t preview the result using the eye icon (👁). Scroll the pages using the nc bar and you will see the header appear on each one. Before continuinc customizations, toggle again to the design mode by clicking the pencil*

Now that the details header is developed, let's continue customizing the details fields. In particular, we are going to add alternate row coloring. To complete the task, select the `Details` item from the `Structure` tab in the Report explorer and search for the `bg-color` property in the `text` group of the `Style` tab. In this case, we are going to evaluate the property with a dynamic expression. To set up the **expression**, click on the green plus icon (⊕), close to the property. At this stage, a configuration window will appear. In the `Formula` text field, in the bottom right of the window, digit the following formula: =IF(ISEVEN(ROWCOUNT()); "white"; "#d2e8f4")

As final tuning, change the `v-align` property to the `MIDDLE` value. Now that the customizations are complete, let's see what the report looks like by previewing the report using the eye icon (👁):