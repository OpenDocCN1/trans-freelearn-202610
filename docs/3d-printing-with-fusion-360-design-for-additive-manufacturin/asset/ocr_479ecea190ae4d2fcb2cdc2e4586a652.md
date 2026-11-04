![](bbox=[0.11, 0.09, 0.85, 0.43])

Figure 14.9 – Saving a custom input as the user default

Another useful option within the same menu is the ability to copy the parameter name of any input field. Figure 14.10 shows the **General** tab of the **SOLID VOLUME SUPPORT** dialog. As a reminder, you can access this dialog by selecting the **Solid Volume Support** command from the **SUPPORTS** panel of the **ADDITIVE** tab, for an additive setup with an FFF 3D printer.

As you can see in the following figure, this tab has two input fields – **Top Distance to Part** and **Bottom Distance to Part**. The default inputs for these fields are 0 . 3 millimeters. Each input field has a parameter name associated with it, which you can reuse within any dialog in the **MANUFACTURE** workspace and while creating scripts or add-ins using Fusion’s API. These parameter names can also be utilized as a part of an equation either within the **Edit Expression** dialog or while entering inputs into other input fields.

Figure 14.10 shows how we can simply copy the parameter name, as shown in callout 1, and paste it into a different input field, as shown in callout 2, to create an equation where are the **Top Distance to Part** input is half the value of the **Bottom Distance to Part** input.