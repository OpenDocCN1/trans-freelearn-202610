If you select **General** on the browser, you will be able to modify the input for **Global Settings**, **Nozzle Priming Settings**, and **Additional G-Code Settings.**

> **Important note**
> Please keep in mind that not all 3D printers are capable of managing all these inputs. For example, if you have a printer without a heated print bed, the **Print Bed Temperature** field will have no effect on your print.

# General

The **General** section of an FFF print setting is where you can modify some of the most common input fields when customizing your print profiles. *Figure 9.4* shows all the input fields you can customize within the **General** section for an FFF print setting. This section is divided into three categories: **Global Settings**, **Nozzle Priming Settings**, and **Additional G-Code Settings**. In the following subsection, we will go over some of the critical input fields within each of these categories and their use cases.

# *Global Settings*

The **Global Settings** subsection located within the **General** section of **Print Setting Editor** is probably the most important part of the print settings. This is where you can edit things such as the build plate temperature and control the height of each layer as well as your first layer. You can also adjust how fast your extruder will move during printing and decide whether you want to print your parts as they’re designed or only their shell using the vase mode. Let’s take a moment and go through each one of these options and their role in generating the additive toolpath:

+   - **Print Bed Temperature (Temp C)**: This is a material-specific input that sets the temperature of the print bed before starting the printing process. A heated bed helps with print bed adhesion of the first layer. In Figure 9.4, the print bed temperature is set to 60°C, which is the recommended bed temperature for printing with a PLA filament. If you were using ABS, this input would be 85°C.
+   - **Layer Height (mm)**: This input specifies the height of each layer. You can reduce the value for better print quality or increase it for a faster print. The upper and lower limits of layer height are dependent on your nozzle size.
+   - **Layer 1 Height (mm)**: This input specifies the height of the first layer of your 3D print. Typically, this input should be 50 to 75% of your nozzle diameter to achieve a successful print bed adhesion on the first layer.
+   - **Travel Speed (mm/s)**: This input specifies the speed with which your extruder will be moving when traveling between points while not extruding filament. In general, you would want this input to be as close to the maximum speed your printer can handle to reduce print time.