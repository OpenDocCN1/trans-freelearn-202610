
![](img/baa027bed101c08b496f1f420cee8a5b_366_0.png)

Figure 12.17 – The three shape outcomes for the top part support connection with polyline supports

The next support type we will highlight in this section is **Volume** support. You can access the **VOLUME SUPPORT** dialogue by selecting its corresponding option within the **SUPPORTS** panel of the **ADDITIVE** tab. Volume supports are very similar to polyline supports. The main difference is that volume supports create multiple polylines to create a stronger support structure. *Figure 12.18* shows the outcome of selecting a single face and creating a volume support. The same figure shows the previously created polyline supports on the left-hand side and the newly created volume supports on the right-hand side. As you can see, we can create structures with **Hole Pattern** set to **Oval** using the **Volume Properties** tab. We can also control **Fragment Size** and **Fragment Gap** so that we have large fragments of supports with contour and a raster gap of 0.25 mm to replicate the shape and size of the polyline supports we previously generated:

![](img/baa027bed101c08b496f1f420cee8a5b_367_0.png)

Figure 12.18 – Creating volume support for an MPBF additive setup

By default, all support structures in Fusion 360 connect selected downskin surfaces to the build plate or an upskin region of a body. Figure 12.19 shows an updated setup, where both Polyline Support1 and Volume Support1, which we created in the previous steps, are suppressed. In this figure, we can see the outcome of selecting multiple surfaces and generating a new Volume support with default settings. Here, we can see that the supports on the left-hand side reach down to the build plate, but the supports on the right-hand side stop on the up skin of the same part. Such support geometry is desirable if we are looking to minimize the time and material we use to manufacture our parts with support structures. But we also have to consider the additional effort we will have to exert during the post-processing phase. Specifically, any support structure that touches the upskin of a given part needs to be removed and any blemishes need to be machined away. In such cases, we may want to avoid creating support structures that touch the upskin of our parts:

![](img/baa027bed101c08b496f1f420cee8a5b_368_0.png)

Figure 12.19 – Generating a volume support using default settings on selected surfaces

One way we can generate support that does not touch the upskin of our components is by generating angled volume supports. **Angled Volume Support** is the final tab within the volume support dialogue. Within this tab, we can activate **Angled Support** and designate the pivot point where our volume support will be broken into two segments. As shown in Figure 12.20, by manipulating the on-canvas gizmo, we can control the breakpoint in our volume supports so that the generated support structure is angled away from the downskin:

![](img/baa027bed101c08b496f1f420cee8a5b_368_1.png)

Figure 12.20 – Manipulating the pivot point for an angled volume support

By default, the angled volume support pivot point allows us to control where our support structures will pivot away from the vertical axis. As you can see in Figure 12.21, the bottom half of the support structure is vertical and is pointing toward the build plate. The top section of the same support structure is angled and points toward the selected downskin surfaces:

![](img/baa027bed101c08b496f1f420cee8a5b_369_0.png)

Figure 12.21 – Angled volume support

This outcome avoids creating supports that touch the up skin of our part. If you select the Tilt below angle checkbox within the Angled Volume Support tab of the dialogue, you can also control the bottom half of the support itself. As mentioned previously, the bottom half of the support is vertical by default. However, you can also control its vector by activating the Tilt below angle checkbox.

As was shown in Figure 12.12, there are multiple polyline and volume support options available for MPBF additive manufacturing setups. All these options are variations of the polyline and volume supports and can be applied to different aspects of our models, such as edges and medial lines for small area surfaces. You can also apply secondary polyline supports to surround an existing volume support. Various options to control these support structures are available within their respective dialogues.

Once we are satisfied with how our models are oriented, positioned, and supported, we can generate the additive toolpath and simulate it as we have done for FFF setups and SLA/DLP setups in the previous chapters. Figure 12.22 shows how you can simulate the additive toolpath by selecting the appropriate command within the ACTIONS panel of the ADDITIVE tab. The SIMULATE ADDITIVE TOOLPATH dialogue for an MPBF setup is the same FFF and SLA/DLP setups. We can use the Show Machine checkbox to display or hide the machine platform. We can use the Incremental Move checkbox to display the layer outcome one layer at a time or in preset increments:

![](img/baa027bed101c08b496f1f420cee8a5b_370_0.png)

Figure 12.22 – Simulating the additive toolpath of an MPBF additive setup

Please keep in mind that MPBF toolpaths would take a longer time to calculate/recalculate if you selected the **Show all layers** option within the **Simulate Additive Toolpath** dialogue as MPBF printers have smaller layer heights.

If you just want to see a quick animation of your additive toolpath, you may want to choose to generate and show every 2nd, 5th, 10th, or 100th layer. The additive toolpath simulation shown in Figure 12.22 was generated after suppressing the polyline support as well as the two volume supports that were used previously to demonstrate the support concepts earlier. **Volume Support3** was added to the entire component using default settings to support the entire body.

Once we are done orienting our models, adding support structures, and simulating the toolpath, we can create the necessary machine build file to start the 3D printing job. At the beginning of this section, we created the machine build file for a generic MPBF printer, as shown in Figure 12.7. In Figure 12.23, we can see that using the same **Create Machine Build File** command will result in a similar dialogue with a machine-specific export option. In this setup, we utilized a **One Click Metal** printer. Therefore, the exporter dropdown only contains the relevant file format, which is **OneClickMetal exporter** (.gcode):

![](img/baa027bed101c08b496f1f420cee8a5b_371_0.png)

Figure 12.23 – Exporting the necessary machine build file from an additive setup

Fusion 360 can create machine build files for eight of the nine machine vendors it has in its machine library. As we just demonstrated, you can create a gcode file specific to an OCM machine. You can also generate the following:

- ILT files (*.ilt), which are required for Aconity printers
- LMI Exporter files (*.gcode), which are required for a Kurtz Ersa Alpha 140 printer
- Additive Industries Exporter files (*.zip), which are required for an Additive Industries MetalFAB1 printer
- MTT files (*.mtt), which are required for all Renishaw printers
- SLM files (*.slm), which are required for all SLM printers
- DMG Mori Lasertec files (*.zip), which are required for all DMG Mori printers
- Xact Metal Exporter files (*.cli or *.sli), which are required for Xact Metal printers, depending on the model number

If you are using Fusion 360 alongside your EOS Metal 3D printers, you will need to have a license to software called EOSPrint from your machine manufacturer to submit jobs to your printer. Fusion 360 can be used to create additive setups with your EOS metal printers. You can select your print settings from Fusion’s print setting library, which aligns with all the print settings supplied by EOS. You can add support structures and slice your models in Fusion as well. However, to create the additive toolpath specific to your EOS hardware, you will need to have a license for EOSPrint.

If you have Fusion 360 and EOSPrint software installed and licensed on your computer, you can generate the additive toolpath and even inquire about the estimated build time for your additive setup. You can connect to your specific printer and submit a print job directly to the printer itself. To activate this functionality, you will also need to download and install EOSPRINT for Autodesk Fusion 360. You can locate this app on the Autodesk App Store at https://apps.autodesk.com/FUSION/en/Detail/Index?id=8154614516617688086.

In this section, we highlighted how to generate an orientation study and select a part position and orientation for a successful metal additive print using MPBF printers. We also went over how to create support structures for this additive manufacturing technology.

In the next section, we will discuss how to create setter supports, which are used during the sintering process after printing parts using metal binder jetting technologies.

## Setter supports for a successful sintering process

MPBF is one of the most adopted technologies for creating metal parts using additive manufacturing. However, recently, there have been several machine vendors entering the metal binder jetting field. Desktop Metal has been a leader in this field for many years. HP also recently entered the metal binder jetting field by releasing a new printer named Metal Jet S100. Markforged, which is a company known for making 3D printers that can print using the continuous fiber reinforcement method, also started offering its Digital Metal PX 100 series of printers to serve the metal binder jetting field. Alongside these well-known brands are also some new entries into the field, such as Sinterjet M60 from Sintertek.

As manufacturers start adopting metal binder jet technology in their workflows, we need to highlight how you can create additive setups using Fusion 360 and prepare your prints for these printers. For the most part, creating an additive setup for a binder jetting technology is no different than the additive setup we create for SLS printers. The most important aspect of an additive setup for a binder jet printer is orienting and arranging our parts within the build volume. Such a setup with four components with an HP Metal Jet S100 printer is shown in Figure 12.24.

Some 3D printers (for example, Digital Metal PX100) require us to scale all our models up before slicing. Other printers (for example, HP Metal Jet S100) can automatically handle the scaling of parts onboard the printer.
