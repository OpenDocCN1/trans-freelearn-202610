+   - **Spiral Vase Mode:** When activated, this will print the part in your active setup using one continuous spiral motion to create a hollow, single-walled object. This mode results in fast print times and avoids any travel and retraction moves during 3D printing. If enabled, many of the inputs within the **Body Presets** section will be ignored as they will no longer be applicable.

# Nozzle Priming Settings

The **Nozzle Priming Settings** subsection located within the **General** section of **Print Setting Editor**, as shown in Figure 9.4, controls all the options related to printing a prime line before the extruder starts working on your part. Let's highlight some of the key features you can control around the prime line:

+   - **Nozzle Priming:** Enabled by default, this option will print a single line at the front, the left, or the right-hand side, or the back of your print bed. The location of the priming line will depend on your part arrangement in the additive setup and the availability of unused space on the print bed. Printing a prime line prepares the nozzle for 3D printing. By printing a prime line, you ensure that the filament is extruding correctly before printing your part.
+   - **Prime Edge Clearance Distance (mm):** This input controls how far away the prime line should be from the border of your build plate.
+   - **Prime Maximum Length (mm):** This input controls the maximum length of your prime line. I recommend increasing this number to match the X or Y dimension of your 3D printer's build plate.

![](bbox=[0.19, 0.49, 0.81, 0.89])

Figure 9.4 – The General tab of Print Setting Editor for FFF print settings