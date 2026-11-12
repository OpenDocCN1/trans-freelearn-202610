![](bbox=[0.128, 0.089, 0.867, 0.294])

Figure 20.16 – The Activate Variant function

The dynamic interface is created by some UMG Widget Blueprints that work together and use the variables from **BP_Configurator**.

# # UMG Widget Blueprints

There are five UMG Widget Blueprints used in the Product Configurator interface. For more information about UMG, see <u>Chapter 7, Creating Screen UI Elements</u>.

These are the UMG Widget Blueprints:

+   - **WBP_MainGUI**: The main Widget Blueprint that contains the other widgets.
+   - **WBP_MainSelector**: This is the Widget responsible for reading the Level Variant Sets and creating the corresponding buttons.
+   - **WBP_VariantRibbonSelector**: This Widget is used to show the Variant options of the selected Variant Set.
+   - **WBP_PopupSelector**: This Widget is similar to **WBP_VariantRibbonSelector**, but it is used for camera and environment lighting.
+   - **WBP_Button**: This Widget represents the button used to select a Variant or a Variant Set.