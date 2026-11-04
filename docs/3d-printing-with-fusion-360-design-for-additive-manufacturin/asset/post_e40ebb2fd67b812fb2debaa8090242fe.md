
![](img/baa027bed101c08b496f1f420cee8a5b_341_0.png)

Figure 11.16 – How to utilize expressions for input fields in NC Program

Once you are done customizing your post properties, all you have to do is select the Post button in the bottom-right corner of the NC Program dialog. Fusion 360 will then export a G-code file in the location you have designated with the filename you have entered. Figure 11.17 shows the output of this action, which is a human-readable text file with a .gcode extension. If you open this file using your native text editor app, such as Notepad, and scroll down, you will be able to spot the beginning of layer 79. As you can see in Figure 11.17, a pause print action is introduced with the code M601, as well as a display message with M117, with text that reads Insert Bars. If you print this G-code on this 3D printer, the print will proceed until it reaches layer 79, and it will pause until you press the knob. Then, you insert your carbon fiber bars.

![](img/baa027bed101c08b496f1f420cee8a5b_342_0.png)

Figure 11.17 – Visualizing the G-code posted using the NC Program dialog

In this section, we highlighted how to use Fusion 360 to generate a G-code file. We showcased how you can access the post-process functionality from the SIMULATE ADDITIVE TOOLPATH dialog, as well as the ACTIONS panel within the ADDITIVE tab of the MANUFACTURE workspace. We covered the various options available in the NC Program dialog to convert our additive toolpath to a G-code file so that we can print it. We also highlighted the various post properties available within the NC Program dialog based on our selected 3D printer.

## Summary

In this chapter, we learned all about how to slice our models for an FFF 3D printer. Slicing is an important step in print preparation for additive manufacturing and comes after creating a setup, orienting parts, arranging them on the build platform, and generating support structures. Once our parts are sliced, we can also simulate the additive toolpath.

In this chapter, we learned how to generate and visualize additive toolpaths layer by layer. We went over the various print statistics, such as energy consumption, print time, material usage Fusion 360 displays for FFF printers. With all this information at our fingertips, we have the power to post-process the additive toolpath as is and create a G-code file. We also highlighted how we can change our print settings based on the simulation results and print statistics, enabling us to print our parts with less material and use less energy.

In the final section of this chapter, we learned how to post-process the additive toolpath and create G-code. At the time of G-code creation, we also learned how to customize it. Using these customizations, we learned how to create temperature towers so that we could determine what temperature to print using certain filaments. We also learned how to pause a print in order to insert additional objects, such as hex nuts, magnets, and strengthening rods. We also learned how to change the filament mid-print so that we could print different segments of our parts using different filament colors. Finally, we learned how to automate certain inputs within the **NC Program** dialog, such as the filename, using parameters that are already available within a Fusion document. Using all these techniques, we can confidently go from an additive setup to generating G-code for an FFF printer.

A lot of the fundamentals of positioning and supporting components for metal 3D printers are similar to what we learned for FFF 3D printing. However, there are some process-specific settings we will need to go over for metal printers. In the following chapter, we will switch gears and cover more about 3D printing with metals.

## Part 4:
Metal Printing, Process Simulation, and Automation

This final part of the book is all about metal 3D printing and automation. First, we will introduce the various ways you can use Autodesk Fusion to create an additive setup for a metal 3D print. We will explain how you can utilize fused filament fabrication, binder jetting, directed energy deposition, and metal powder bed fusion technologies to create metal parts. Then, we will demonstrate how to simulate the metal powder bed fusion process in order to detect potential print failures. We will also demonstrate how to compensate for large distortions in our models and ultimately minimize possible print failures.

We will end the book by highlighting various automation techniques that we can utilize to reduce the time spent on repetitive tasks. We will introduce premade automation tools available for download and use on the Autodesk App Store. We will introduce the concept of operation templates as well as scripts created with Python or C++, using Autodesk Fusion’s API, and how to customize them to fit our needs.

This part has the following chapters:

- Chapter 12, 3D Printing with Metal Printers
- Chapter 13, Simulating the MPBF Process
- Chapter 14, Automating Repetitive Tasks

## 12

## 3D Printing with Metal Printers

Welcome to Chapter 12, where we will learn all about metal additive manufacturing. Metal 3D printing has been increasing in popularity over the past several years as it offers a long list of benefits for numerous industries, such as aerospace, healthcare, and high-end consumer goods. Using metal 3D printing, you can create parts with complex internal structures and organic external faces, which are impossible to manufacture any other way. 3D printing with metal is also more cost-effective compared to other manufacturing methods for low quantities.

Fusion 360, along with the Additive Build extension, can be utilized to create additive setups with 3D printers that are capable of manufacturing parts using metals. In this chapter, we will introduce the various ways you can use metal additive manufacturing within Fusion 360. We will go over how to utilize Fusion 360 to prepare a model to be 3D printed using fused filament fabrication, metal binder jetting, metal powder bed fusion, and directed energy deposition techniques. We will also show how to select associated print settings based on the printing technology. We will highlight how to orient and place our parts based on machine-specific factors, such as recoater blade direction.

Depending on the additive technology utilized by our specific 3D printer, as well as our part orientation, parts may need support structures during the printing process. As the supports utilized during metal powder bed fusion are different compared to the supports required for fused filament fabrication and resin printing, we will also learn about how to create technology-dependent support structures to minimize material waste, decrease print time, and minimize post-processing effort.

Whether we are utilizing a customized Fused Filament Fabrication (FFF) 3D printer to print with a metal filament, or we use a metal binder jet 3D printer, we may need support structures to successfully print our parts or to provide the necessary support for our parts during the sintering process. In this chapter, we will also learn how to use Fusion 360 to create setter supports to prevent our parts from deforming due to the high temperatures they will be exposed to within a sintering oven.

In this chapter, we will go over the following topics:

- Introduction to metal 3D printing
- 3D printing with metal powder bed fusion machines
- Setter supports for a successful sintering process

By the end of this chapter, you will have a good understanding of the various 3D printing technologies you can use to manufacture parts using metal materials. You will have learned how to create an additive setup using Fusion 360 with a 3D printer capable of manufacturing a metal part. You will gain an understanding of how to choose an appropriate part placement and orientation based on your 3D printing technology. You will have also learned how to create lightweight support structures for metal 3D printing. Finally, you will have learned how to create support structures for parts printed using metal binder jetting to minimize distortions during the sintering process.

## Technical requirements

In this chapter, we will cover metal additive manufacturing. We will highlight how you can use FFF and binder jetting printers to 3D print in metal. These printers are accessible to all Fusion 360 users (license type: Personal, Trial, Commercial, Startup, and Educational).

However, we will also utilize multiple metal powder bed fusion (MPBF) 3D printers and their respective print settings. We will highlight how to generate support structures for MPBF machines and metal binder jet machines. These support structures are not available with Fusion 360 until you gain access to the Additive Build extension.

As with all other Fusion 360 extensions, the Additive Build extension is not available for personal use.

If you have a Commercial license of Fusion 360, to create an Additive setup with an MPBF machine, you will also need access to the Additive Build extension (https://www.autodesk.com/products/Fusion-360/additive-build-extension).

Autodesk offers a 14-day trial for the Additive Build extension for all commercial users of Fusion 360. You can activate your trial within the extensions dialog of Fusion 360.

The Additive Build extension is also available for free to Startup and Educational license users.

The lesson files for this chapter can be found here: https://github.com/PacktPublishing/3D-Printing-with-Fusion-360.

## Introduction to Metal 3D printing

In Chapter 7, we created our first additive setup using an FFF printer. In this chapter, we will highlight how to create additive setups with machines that are capable of 3D printing with metal.

The Fusion 360 machine library includes machines with additive capabilities for various technologies. Figure 12.1 shows how we can filter additive machines based on their technology within the machine library. Fusion 360’s machine library can be accessed within the MANUFACTURE workspace. You can access the Machine Library command within the MANAGE panel of any tab. If you have access to the Additive Build extension, you will be able to select an MPBF machine within Machine Library while creating an Additive setup:
