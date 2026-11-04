![](bbox=[0.11, 0.09, 0.85, 0.41])

Figure 9.30 – Simulation of the additive toolpath

The preceding figure shows the slice 46.55 millimeters above the build platform. The **Show Machine** checkbox has been deselected within **SIMULATE ADDITIVE TOOLPATH** to hide the build plate. The view is set to **TOP** from the View cube. **Preview Mode** is set to **Display current layer only**. This combination of settings shows us the toolpath for the current layer and hides everything else. The horizontal box that we see in the preceding figure represents the body that will be printed using the red filament with **Extruder 2**. The vertical box that we see in the same figure represents the body that will be printed using the yellow filament with **Extruder 1**. You will notice that these two boxes have a distinct perimeter around them as they are two different bodies that are being printed with two different extruders. This means that if you print this toolpath, the interface between these two boxes will be more pronounced versus if you were to print them using the same body preset or as a single joined body with a uniform perimeter and a continuous infill.

# Summary

In this chapter, we focused on print settings, including how to customize them and how to assign unique body presets to given parts. This was a long chapter so let's do a quick recap of what we learned.

Fusion 360 has a library of print settings you can use when creating your additive setups. Each print setting has a set of default values that you can use for printing with your specific printer and material combination. However, there will be instances where you may want to modify some of these settings to either improve your print quality or fit the needs of your specific printer. In such cases, you can select the print setting you wish to modify from Fusion 360's library and copy it over to your personal Print Setting Library folders. Once you have a print setting in your local library, you can modify it