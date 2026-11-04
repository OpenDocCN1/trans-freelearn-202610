+   - **Flip Image**: Depending on the placement and the model of the projector (LCD versus DLP) your 3D printer is using, you may need to export mirror images for all your slices so that you don't 3D print a mirror image of the object.
+   - **PNG Filename Prefix**: Different 3D printers have different requirements for how PNG files should be named. Using this input field, you can customize how each PNG is named by adding a prefix. If you simply keep the existing prefix of `image_`, the resulting PNG files will be named `image_000.png`, `image_001.png`, and so on.

![](bbox=[0.22, 0.24, 0.75, 0.75])

Figure 9.18 – Autodesk Generic DLP print settings

After creating an additive setup with an Autodesk Generic SLA/DLP printer and the **Autodesk Generic DLP** print setting, if you select **Create Machine Build File** located within the **ACTIONS** panel of the **ADDITIVE** tab, you will be able to export each slice as a PNG file with your specifications, compressed as a single ZIP file. You can then further edit the ZIP file to include any additional TXT or XML files your printer may need before transferring it to your 3D printer for manufacturing your models.