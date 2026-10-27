
Do you want to create your own seamless textures? Have a look at the tutorials that you can find on CGTextures.com. A few that I recommend are as follows:

- Photoshop Match Color
- Photoshop tricks and tips
- Tiling complex patterns
- Recoloring wood textures

After creating a seamless or tiled texture, we can create an alpha channel to add more detail. This alpha channel is used as a reflection mask in Lumion, blocking areas where we don't want the light to reflect, as you can see in the following screenshot:

![](img/192b6e55337adc00931f510ecaaf8e05_69_0.png)

Can you see the lines in the texture?

## Creating Materials

By the way, in the **Standard** material in the **Properties** sub-menu, there is a slider called **Reflection/Clip**. This allows you to use the alpha channel to clip your model, as seen in the following example. I use a grid where the white lines show the material and the black areas hide the material. And this is the way an alpha channel works; white reveals detail and black hides it. The following screenshot shows an alpha map in use:

![](img/192b6e55337adc00931f510ecaaf8e05_70_0.png)

I use an alpha texture to create this grid

The final thing that you need to create is a normal map, so you can use the **Bump** slider in Lumion to add detail. To create a normal map using Adobe Photoshop or Gimp, we need to install a plugin.

> Want to know how to create a normal map? Have a look at the following URLs:
> 
> - **NVIDIA Texture Tools for Adobe Photoshop**: `https://developer.nvidia.com/nvidia-texture-tools-adobe-photoshop`
> - **Gimp-normalmap**: `https://code.google.com/p/gimp-normalmap/`

## Summary

In this chapter you learned how to use and configure the prebuilt materials in Lumion. You found out how to use the **Landscape** material to create a terrain and some useful materials such as the **Invisible** and **Water** materials. In the final section, you discovered some places where you can find free textures and learned what you need to create a seamless **Diffuse** texture, alpha channel, and normal maps. In the next chapter, you will learn how to export your 3D model from a 3D application such as SketchUp and import it without problems in Lumion.

## 4

## Working with Lumion

It’s time to start working with Lumion, and this chapter will help you create a bridge between the 3D application of your choice and Lumion. Exporting the 3D model and bringing it into Lumion can cause problems at times. To start on the right foot, we are going to analyze some important aspects before exporting the 3D model. Once the model is exported into Lumion, we will learn how to place and adjust the model. This chapter has been written with the intention of helping you get everything you need so that you can start working in Lumion without any delay.

In this chapter, you will learn the following topics:

- What is COLLADA?
- Exporting from SketchUp
- Exporting from 3ds Max
- Exporting from other 3D software
- Importing the model
- Working with layers
- Placing the model

As you can see from the introduction, this chapter is more technical, but at the same time it is very important as you will learn the right way to export your 3D model here. To get a smooth workflow, you need to pay close attention to this section. To answer the question: what is the best format to use? COLLADA is the perfect answer.

## What is COLLADA?

COLLADA is an XML namespace and database schema used to create a bridge between the different 3D tools, making an easy sharing of 3D geometry, shaders, and effects possible. In plain English, it allows you to move 3D files between SketchUp, 3ds Max, Maya, Revit, and Lumion. However, nothing stops you from using other file formats such as the following:

- Autodesk RealDWG - *.dxf
- Autodesk RealDWG - *.dwg
- FBX file - *.fbx
- Max file - *.max
- 3ds file - *.3ds
- Obj file - .obj

The reason behind using COLLADA is that it is a better option compared to the file formats we previously mentioned. As I said, there is nothing that's going to stop you from using those file formats, but they are less reliable and do not always include all the data we want. To get the best results with a low possibility of error, COLLADA is the right choice. COLLADA is not available in all the applications, and in some cases, such as when we are using an older version of 3ds Max or Maya, we need to install a COLLADA plugin called OpenCOLLADA.

Download OpenCOLLADA at https://code.google.com/p/opencollada/downloads/list.

The Lumion team has done a good job of creating a COLLADA plugin called COLLADA exporter for Revit. The downside of this plugin is that it doesn't work with the Revit LT version, and the only versions available are Revit 2012 and 2013.

Download COLLADA exporter at http://lumion3d.com/lumion-exporter-for-autodesk-revit.

There are some workarounds when using other applications such as ArchiCAD. According to the Lumion team, if you are using this software, the best solution is to export your model as a SketchUp file. SketchUp will be used as an intermediary between ArchiCAD and Lumion.

> This ArchiCAD add-on is available for download at http:// www.graphisoft.com/products/archicad/addons/ google_earth/.

I recommend that you use SketchUp as the middleman if your 3D software doesn't have the option to export a COLLADA file. Now that we know what COLLADA is and why it's so important to learn its usage, let's have a look at how we can export a 3D model from various 3D software programs.

## Exporting from SketchUp

Lumion 4 allows you to directly import a SketchUp file. However, if you have a previous Lumion version, this is what you need to do. Exporting your model using SketchUp is the simplest thing, and in Chapter 1, What is Lumion?, we had a quick look at how to do it, but let's use some pictures now to help you understand all the steps better. In SketchUp, select the File menu, hover your mouse over the Export option, and select 3D model.

Now, you need to set some options to export the 3D model correctly. Click on the Options button and select the following options:

- Triangulate All Faces
- Preserve Component Hierarchies
- Export Texture Maps

![](img/192b6e55337adc00931f510ecaaf8e05_75_0.png)

The correct settings to export your model as a COLLADA file

### Working with Lumion

You need to select the **Triangulate All Faces** option to be able to see your 3D model inside Lumion, and the **Export Texture Maps** option will create a folder with all the texture maps in the same folder where you exported the 3D model. Don't forget to assign different materials to each section of your model because the number of materials that you export with your 3D model is equal to the number of materials that you can change inside Lumion. Moving forward, let's see how we can perform the same procedure using 3ds Max.

## Exporting from 3ds Max

To work with 3ds Max, we need to take into account a few aspects before exporting the 3D model. Don't forget to use real dimensions when modeling with 3ds Max; otherwise, your 3D model will be very tiny or really big once you import it in Lumion. There are a few more things you need to pay more attention to.

Before exporting materials, convert them to standard materials because if you use any other type of material when importing to Lumion, such as V-ray materials, the model will be imported without any textures. Also, when using a standard material, you can use two textures: one in the **Diffuse** slot and the other in the **Self-illumination** slot to use lightmaps or give additional details. Besides these two options, anything that you may have in the material, for example, a specular map or bump map, will not be exported.

Now it's time to export your model. Choose **OpenCollada**, or if you are using a more recent version of 3ds Max, you can choose **Autodesk COLLADA**. If you are using this option, select **Convert Deforming Dummies to Bones in the Geometry** tab and **Triangulate** and **Single Matrix** in the Collada tab.
