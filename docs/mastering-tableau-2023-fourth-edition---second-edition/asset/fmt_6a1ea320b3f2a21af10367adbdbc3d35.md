
![](Images/B18435_09_59.png)

Figure 9.59: XX(will be *Figure 9.59*)

So why this effort? Because a grid map distributes the states evenly, if you did this without it, you wouldn’t be able to distinguish between states in close proximity and would miss some far away:

![](Images/B18435_09_60.png)

Figure 9.60: If no grid map

Thank you Klaus for this great tip! By the way, a hex map is not only applicable to the U.S.. Try it with Europe, Africa, South America, or the **Asia-Pacific** (**APAC**) region as well! Whenever you work with variances in size, a grid map comes in handy.

## Summary

In this chapter, we explored how to extend Tableau’s mapping capabilities without leaving the interface by capturing Tableau-generated latitude and longitude data and how to look this up on Google. We created polygons and a heatmap for Texas based on a `.shp` file as well as a dashboard with a dual map axis and another dashboard with 4 map layers. Next, we explored various ways to extend Tableau’s mapping using other technology. We connected to a WMS server and then explored the Mapbox offering, followed by an excursion to the world of polygons.

In the next chapter, we will explore using Tableau for presentations. Specifically, we will look at how to get the best images out of Tableau, how to effectively and efficiently use Tableau with PowerPoint, and how to use Tableau directly for presentations without relying on third-party tools.

## Learn more on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

[https://packt.link/tableau](https://packt.link/tableau)

![](Images/QR_Code2044012316175764640.png)

# 10 Presenting with Tableau

All Tableau authors are essentially storytellers. Analyzing data is more than just puzzle-solving; it is a search for a story that will make a difference. Topics can range from Airbnb to the Zika virus, and may be pleasantly diverting or life-changing, but they all serve a common need: to tell a story. This chapter is dedicated to helping you stock your toolkit of knowledge with ideas and methods for using Tableau to make presentations that engage, delight, and make a difference.

This chapter will explore the following presentation-centric topics:

*   Getting the best images out of Tableau
*   From Tableau to PowerPoint
*   Embedding Tableau into PowerPoint
*   Animating Tableau
*   Story points and dashboards for presentations
*   Presentation resources

## Getting the best images out of Tableau

In this section, we will review options for exporting an image from Tableau into other applications and discuss the pros and cons of each method. We’ll begin by surveying various screenshot applications and then we will consider methods that are available directly in Tableau.

Perhaps the easiest way to capture an image in Tableau is to use a screen capture tool. The following are some screen capture tools that won’t impact your pocketbook:

*   **Grab** is a screen capture utility natively available on macOS. Grab is located in the **Utilities** folder under **Applications**. You can also access it via the following shortcuts: *Shift* + *Command* + *3* for a screenshot, *Shift* + *Command* + *4* for a partial screenshot, and *Shift* + *Command* + *5* for a timer screenshot.
*   **Snipping Tool** is installed by default with Windows, a very simple tool with few bells and whistles, easy to use and effective.
*   **Greenshot** is an open-source screen capture tool with many features. Visit [http://getgreenshot.org/](http://getgreenshot.org/) to download the application and learn more.
*   **Microsoft Office OneNote** includes a screen capture feature. If you have OneNote installed, simply press *Windows* + *S* to activate the screen capture or else press *Windows* + *N* and then the *S* key for activation.

**General rule of thumb**: Capture a screenshot bigger than is needed because capturing more pixels will increase the quality. You can then decrease the size and make it fit for purpose, keeping more pixels on a smaller surface. (For very small objects, increase the scale of your computer display temporarily to get more pixels per square centimeter.)

Now that we have covered the tools for image capturing, let us have a look into the native capabilities that ship with Tableau.

### Tableau’s native export capabilities

One of the shortcomings of screen capture tools is that they are limited to **raster images**. Raster images are often sufficient for documentation or a **PowerPoint** presentation but are subject to pixelation if enlarged. Vector images, on the other hand, do not pixelate when enlarged and may therefore provide sharper image quality. Natively, Tableau includes both raster and vector export options. These options are discussed in the following list.

Tableau can export images in three formats, accessible via `Worksheet | Export | Image`:

*   **SVG** is the only vector image offered in Tableau at the time of writing. Use this format to avoid pixelation if enlarged. If your image has lots of fine details and textures, SVGs are at a disadvantage because they are based on points and paths rather than single pixels.
*   **JPEG** is a raster image format that is good for high-resolution images such as photographs, but does not work very well for low-color images, such as those typically deployed in Tableau. Export an image from Tableau in JPEG format and then zoom in close. Note that the white space (especially white space surrounding text) includes stray pixels of various colors. These are known as **artifacts** or **noise**. Although these pixels are not visible unless zoomed in, the overall impact on an exported image is that it can look blurry. Thus, there is rarely, if ever, a reason to export to JPEG from Tableau.
*   Like JPEG images, **PNG** images are raster. The advantage of the PNG format is that it works well with both high-color images such as photographs and low-color images like those typically used in Tableau. Export an image from Tableau in PNG format and zoom in to observe that, although pixelation occurs, the white space comprises only white. Unlike JPEG images, no artifacts or noise appear. PNG should be considered the format of choice when using a raster image.
*   **BMP** is a raster image format that looks quite nice but is uncompressed and can thus result in large image files. Today, the BMP format is considered antiquated and should typically be avoided.

I suggest you try all four of them. Luckily, Tableau also provides us with a native feature to export to a PDF as a vector image, as well as exporting to PowerPoint directly. In the next section, we will take a look at the PowerPoint export feature and some variations to it.

## From Tableau to PowerPoint

**PowerPoint** is ubiquitous. Some may argue that other presentation tools such as Prezi are superior, but for many organizations (probably the vast majority) PowerPoint remains the software of choice.

Prezi is a tool that can be accessed via the web. Typically, you won’t see classical slides but an interactive, moving presentation. Templates are available online and they offer free as well as paid tiers. You can check it out here: [www.prezi.com](http://www.prezi.com).

As such, it’s important to integrate Tableau and PowerPoint efficiently and effectively. It is possible to export your dashboard directly into PowerPoint. You will find the option under `File | Export As PowerPoint…`:

![](Images/B18435_10_01.png)

Figure 10.1: Export As PowerPoint…

After making this selection, you will be asked whether you want to export the current view or a selection of sheets from the workbook:

![](Images/B18435_10_02.png)

Figure 10.2: Export PowerPoint
