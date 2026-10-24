
## Table of Contents

- Sampling a raster layer 628
- Visualizing multispectral layers 630
- Modifying and reclassifying values in raster layers 634
- Performing supervised classification of raster layers 637
- **Chapter 9: QGIS and the Web** 641
    - Introduction 641
    - Using web services 642
    - Using WFS and WFS-T 644
    - Searching CSW 647
    - Using WMS and WMS Tiles 649
    - Using WCS 653
    - Using GDAL 656
    - Serving web maps with the QGIS server 660
    - Scale-dependent rendering 665
    - Hooking up web clients 669
    - Managing GeoServer from QGIS 674
- **Chapter 10: Cartography Tips** 677
    - Introduction 677
    - Using Rule Based Rendering 678
    - Handling transparencies 684
    - Understanding the feature and layer blending modes 687
    - Saving and loading styles 691
    - Configuring data-defined labels 695
    - Creating custom SVG graphics 700
    - Making pretty graticules in any projection 704
    - Making useful graticules in printed maps 709
    - Creating a map series using Atlas 713
- **Chapter 11: Extending QGIS** 717
    - Introduction 717
    - Defining custom projections 718
    - Working near the dateline 722
    - Working offline 727
    - Using the QspatialLite plugin 729
    - Adding plugins with Python dependencies 731
    - Using the Python console 733
    - Writing Processing algorithms 736
    - Writing QGIS plugins 740
    - Using external tools 745

## Table of Contents

- **Chapter 12: Up and Coming** — 751
    - Introduction — 751
    - Preparing LiDAR data — 752
    - Opening File Geodatabases with the OpenFileGDB driver — 755
    - Using Geopackages — 757
    - The PostGIS Topology Editor plugin — 760
    - The Topology Checker plugin — 764
    - GRASS Topology tools — 768
    - Hunting for bugs — 772
    - Reporting bugs — 776
- **Bibliography** — 781
- **Index** — 781

## Module 1

# Learning QGIS, Third Edition

The latest guide to using QGIS 2.14 to create great maps and perform geoprocessing tasks with ease

## 1

### Getting Started with QGIS

In this chapter, we will install and configure the **QGIS** geographic information system. We will also get to know the user interface and how to customize it. By the end of this chapter, you will have QGIS running on your machine and be ready to start with the tutorials.

### Installing QGIS

QGIS runs on **Windows**, various **Linux** distributions, **Unix**, **Mac OS X**, and **Android**. The QGIS project provides ready-to-use packages as well as instructions to build from the source code at http://download.qgis.org. We will cover how to install QGIS on two systems, Windows and Ubuntu, as well as how to avoid the most common pitfalls.

> Further installation instructions for other supported operating systems are available at http://www.qgis.org/en/site/forusers/alldownloads.html.

Like many other open source projects, QGIS offers you a choice between different releases. For the tutorials in this book, we will use the **QGIS 2.14 LTR** version. The following options are available:

- **Long-term release (LTR):** The LTR version is recommended for corporate and academic use. It is currently released once per year in the end of February. It receives bug fix updates for at least a year, and the features and user interface remain unchanged. This makes it the best choice for training material that should not become outdated after a few months.
- **Latest release (LR)**: The LR version contains newly developed and tested features. It is currently released every four months (except when an LTR version is released instead). Use this version if you want to stay up to date with the latest developments, including new features and user interface changes, but are not comfortable with using the DEV version.
- **Developer version (DEV, master, or testing)**: The cutting-edge DEV version contains the latest and greatest developments, but be warned that on some days, it might not work as reliably as you want it to.

> You can find more information about the releases as well as the schedule for future releases at http://www.qgis.org/en/site/getinvolved/development/roadmap.html#release-schedule. For an overview of the changes between releases, check out the visual change logs at http://www.qgis.org/en/site/forusers/visualchangelogs.html.

#### Installing QGIS on Windows

On Windows, we have two different options to install QGIS, the standalone installer and the **OSGeo4W** installer:

- The **standalone installer** is one big file to download (approximately 280 MB); it contains a QGIS release, the **Geographic Resources Analysis Support System (GRASS)** GIS, as well as the **System for Automated Geoscientific Analyses (SAGA)** GIS in one package.
- The **OSGeo4W installer** is a small, flexible installation tool that makes it possible to download and install QGIS and many more OSGeo tools with all their dependencies. The main advantage of this installer over the standalone installer is that it makes updating QGIS and its dependencies very easy. You can always have access to both the current release and the developer versions if you choose to, but, of course, you are never forced to update. That is why I recommend that you use **OSGeo4W**. You can download the 32-bit and 64-bit OSGeo4W installers from http://osgeo4w.osgeo.org (or directly from http://download.osgeo.org/osgeo4w/osgeo4w-setup-x86.exe for the 32-bit version or http://download.osgeo.org/osgeo4w/osgeo4w-setup-x86_64.exe if you have a 64-bit version of Windows). Download the version that matches your operating system and keep it! In the future, whenever you want to change or update your system, just run it again.

> 💡 Regardless of the installer you choose, make sure that you avoid special characters such as German umlauts or letters from alphabets other than the default Latin ones (for details, refer to `https://en.wikipedia.org/wiki/ISO_basic_Latin_alphabet`) in the installation path, as they can cause problems later on, for example, during plugin installation.

When the OSGeo4W installer starts, we get to choose between **Express Desktop Install**, **Express Web-GIS Install**, and **Advanced Install**. To install the QGIS LTR version, we can simply select the **Express Desktop Install** option, and the next dialog will list the available desktop applications, such as **QGIS**, **uDig**, and **GRASS GIS**. We can simply select **QGIS**, click on **Next**, and confirm the necessary dependencies by clicking on **Next** again. Then the download and installation will start automatically. When the installation is complete, there will be desktop shortcuts and start menu entries for OSGeo4W and QGIS.

To install QGIS LTR (or DEV), we need to go through the **Advanced Install** option, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_23_0.png)

This installation path offers many options, such as **Download Without Installing** and **Install from Local Directory**, which can be used to download all the necessary packages on one machine and later install them on machines without Internet access. We just select **Install from Internet**, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_24_0.png)

When selecting the installation **Root Directory**, as shown in the following screenshot, avoid special characters such as German umlauts or letters from alphabets other than the default Latin ones in the installation path (as mentioned before), as they can cause problems later on, for example, during plugin installation:

![](img/2e88bf5938bf48a409c0a534a8775b21_24_1.png)

Then you can specify the folder (**Local Package Directory**) where the setup process will store the installation files as well as customize **Start menu name**. I recommend that you leave the default settings similar to what you can see in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_25_0.png)

In the Internet connection settings, it is usually not necessary to change the default settings, but if your machine is, for example, hidden behind a **proxy**, you will be able to specify it here:

![](img/2e88bf5938bf48a409c0a534a8775b21_25_1.png)
