
![](img/baa027bed101c08b496f1f420cee8a5b_264_0.png)

Figure 8.26 – Arrangement outcome of duplicate parts created using a rectangular pattern

Alternatively, we could have oriented all the components using the Minimize Build Height command in the POSITION panel of the ADDITIVE tab, as shown in Figure 8.27. Please note that the following screenshot demonstrates the outcome of this workflow after having created a new manufacturing model (Manufacturing Model 3) and a new setup (Setup3) that uses the new manufacturing model as input.

![](img/baa027bed101c08b496f1f420cee8a5b_264_1.png)

Figure 8.27 – Minimizing build height and 3D arranging all components

Once the parts are in an orientation where they occupy the least amount of Z height, we can perform the Additive Arrange functionality. Figure 8.27 shows the outcome of Additive Arrange after having reoriented the parts using the minimized build height. You can see that this outcome has a much higher arrangement density based on the printer utilization. If needed, we could also manually move the components after the automatic ADDITIVE ARRANGE action is complete. Sometimes automated solutions cannot identify the best outcome, whereas, as manufacturing engineers, we can visually spot certain part orientations and positions that will benefit the overall arrangement density.

## Summary

In this chapter, we talked about how to position and arrange our components for 3D printing. We started the chapter by showcasing how to translate and rotate our components manually using the move commands available within the MANUFACTURE workspace in the context of an additive setup. We also covered how to automatically orient our parts using explicit functions such as Minimize Build Height and Automatic Orientation. We talked about the various ranking options available within the AUTOMATIC ORIENTATION dialog so that the outcomes of an orientation study are ranked based on the needs of our additive technology. We ended the chapter by demonstrating how to arrange our components within the 3D build volume of our printer. In the final section, we covered how to use the automated arrange tools for existing orientations of components. We explained how we can orient and arrange our components within the DESIGN workspace, as well as the MANUFACTURE workspace in the context of an additive setup. We highlighted the benefits of using Additive Arrange in the MANUFACTURE workspace and talked about how to combine part orientation and part arrangement to achieve a higher arrangement density.

Now that we know how to arrange and orient our parts for 3D printing, we can start focusing on how to print each component with unique process parameters. In the next chapter, we will cover how to assign unique print settings to each component in order to modify how each component is printed, thereby impacting its physical properties.

## Print Settings

Welcome to Chapter 9. In the previous chapters, we learned how to model for 3D printing and how to create an additive setup and position our components within that setup. We talked about effective ways to orient our parts and utilize 3D arrangement tools to get the most out of our 3D printer.

In this chapter, we will focus on print settings. We will highlight how to modify certain inputs within the print settings and their impact on the model that is being printed. We will also cover different print settings for different additive manufacturing technologies. Finally, we will touch upon how to assign a unique print setting on different parts within the build volume of our printer so that we can control how each part is printed.

Most people start their 3D printing journey by printing one component at a time. As they get more comfortable with 3D printing, they move on to printing multiple components in a single print. An important step to gaining that confidence relies on a better understanding of how to fine-tune our print profiles so that we can get reliable and successful prints from our 3D printer.

This phase of learning how to 3D print consistently will likely require some experimentation on your end with your specific 3D printer. The subjects we will cover in this chapter will make it easier for you to experiment with various print settings and dial in your print profiles for your specific printer and material.

In this chapter, we will cover the following topics:

- Overview of print settings for fused filament fabrication
- Overview of print settings for SLA and DLP printing
- Assigning unique print settings to bodies

In some cases, it may be necessary to edit the selected print setting to change a certain aspect of the printing process. For example, if you are experiencing print failures due to your parts not sticking to the build plate, you may want to add a skirt or a brim to improve print bed adhesion. If your 3D-printed parts are not solid enough, you may want to increase the infill density or the number of perimeters. In some cases, you may want to use a different print setting for a different part of the print. All of these and more will be explored in this chapter.

By the end of this chapter, you will have a better understanding of what makes a print setting in Fusion 360. You will learn that most print settings have multiple presets and you will understand the differences between those presets. You'll learn how to create your own presets and customize existing ones by changing certain print settings. You'll gain a solid understanding of key print settings and their impact on the additive toolpaths and slices Fusion 360 creates when generating the machine file. You will also learn how to assign print settings per body within a component and the impact on the additive toolpath.

## Technical requirements

All topics covered in this chapter are accessible for Personal, Trial, Commercial, Startup, and Educational Fusion 360 license types.

Certain Fusion 360 additive machines and print settings, such as the metal powder bed fusion (MPBF) 3D printers and their respective print settings, are not available until gaining access to the Additive Build Extension. As with all other Fusion 360 extensions, the Additive Build Extension is not available for Personal use. As we will not cover topics specific to metal 3D printing in this chapter, you should be able to follow along with all the examples in this chapter with no issues.

If you have a Commercial license of Fusion 360, to create an additive setup with an MPBF machine, you will also need access to the Additive Build Extension (https://www.autodesk.com/products/Fusion-360/additive-build-extension).

Autodesk offers a 14-day trial for the Additive Build Extension for all Commercial users of Fusion 360. You can activate your trial within the Extensions dialog of Fusion 360.

The lesson files for this chapter can be found here: https://github.com/PacktPublishing/3D-Printing-with-Fusion-360.

## Overview of print settings for Fused Filament fabrication

In this first section, we will focus our efforts on print settings that are specific to fused filament fabrication (FFF) additive manufacturing. To demonstrate how to access and edit print settings for FFF, we will be using a CAD model named Knobs, as shown in Figure 9.1. This part is actually the dial knob for a Prusa i3 MK3S+ 3D printer and is available to download as a mesh object on printables.com as one of the 38 components uploaded by Prusa Research as replacement parts for this 3D printer (https://www.printables.com/model/57217-i3-mk3s-printable-parts/). So, if you wanted to, you could download and print this part with a different colored filament or a different material and swap out your existing knob on your printer.

In this example, we will be using a solid (not the original mesh) version of this part. After opening the part in Fusion 360 and switching to the **MANUFACTURE** workspace, we can create an additive setup using the **Prusa i3 MK3S+** printer and the **PLA (Direct Drive)** print setting, as shown in Figure 9.1. If we expand the print setting, we will notice that the preset that is assigned to this part is named **Normal**. If we want to edit a print setting or a preset, we can do so by right-clicking on the print setting within the active setup and selecting **Edit**. Another way is to go to **Print Setting Library** from the **MANAGE** panel. Within **Print Setting Library**, we can select the print setting we wish to customize from **Fusion 360 library**, and copy and paste it into either our **Cloud**, **Local**, or **Linked** library, as shown in Figure 9.1.

![](img/baa027bed101c08b496f1f420cee8a5b_269_0.png)

Figure 9.1 – Managing print settings within Print Setting Library

Once a print setting is saved under a folder within the **My print settings** section of **Print Setting Library**, we can select it, right-click on it, and edit it for further customization, as shown in Figure 9.2.
