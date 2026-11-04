If you switch over to the **Ranking** tab within the **AUTOMATIC ORIENTATION** dialog, you will be able to control how Fusion 360 lists the outcomes of the orientation study, as shown in Figure 8.12. While generating a list of orientations for the selected component, Fusion 360 also calculates additional information such as the following:

+   - How much support would be necessary to print an object in a given orientation
+   - The total surface area on the parts where supports would be necessary based on the support overhang angle input
+   - What the bounding-box volume would be in a given orientation
+   - The maximum Z height occupied in a given orientation
+   - The height of the center of gravity in a given orientation

Using the dropdowns next to each of these ranking criteria, you can assign a priority to the values calculated by Fusion 360 for each item. As previously mentioned, the default ranking priority is the preferred ranking priority to use for both FFF and MPBF technologies.

![](bbox=[0.125, 0.42, 0.87, 0.74])

Figure 8.12 – Ranking criteria for an automatic orientation study

After conducting an orientation study and clicking the **OK** button in the **AUTOMATIC ORIENTATION** dialog, Fusion 360 adds a new line item called Automatic Orientation1 within the **Orientations** folder in the browser for the active additive setup, as shown in Figure 8.13. Once Fusion 360 generates all the orientation outcomes, it will display the available orientation to choose from in a new tab called **Results** within the **ORIENTATION RESULTS** dialog. If you close this dialog by clicking **OK**,