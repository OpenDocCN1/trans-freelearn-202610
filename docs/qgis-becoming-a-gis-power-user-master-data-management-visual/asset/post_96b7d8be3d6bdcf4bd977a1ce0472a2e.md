
Then we can pick the download site. At the time of writing this book, there is only one download server available, anyway, as you can see in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_26_0.png)

After the installer fetches the latest package information from OSGeo's servers, we get to pick the packages for installation. QGIS LTR is listed in the desktop category as qgis-ltr (and the DEV version is listed as qgis-dev). To select the LTR version for installation, click on the text that reads Skip, and it will change and display the version number, as shown in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_26_1.png)

As you can see in the following screenshot, the installer will automatically select all the necessary dependencies (such as GDAL, SAGA, OTB, and GRASS), so we don't have to worry about this:

![](img/2e88bf5938bf48a409c0a534a8775b21_27_0.png)

After you've clicked on Next, the download and installation starts automatically, just as in the Express version.

You have probably noticed other available QGIS packages called qgis-ltr-dev and qgis-rel-dev. These contain the latest changes (to the LTR and LR versions, respectively), which will be released as bug fix versions according to the release schedule. This makes these packages a good option if you run into an issue with a release that has been fixed recently but the bug fix version release is not out yet.

> If you try to run QGIS and get a popup that says, The procedure entry point <some-name> could not be located in the dynamic link library <dll-name>.dll, it means that you are facing a common issue on Windows systems—a DLL conflict. This error is easy to fix; just copy the DLL file mentioned in the error message from C:\OSGeo4W\bin\ to C:\OSGeo4W\apps\qgis\bin\ (adjust the paths if necessary).

## Installing on Ubuntu

On Ubuntu, the QGIS project provides packages for the LTR, LR, and DEV versions. At the time of writing this book, the Ubuntu versions Precise, Trusty, Vivid, and Wily are supported, but you can find the latest information at http://www.qgis.org/en/site/forusers/alldownloads.html#debian-ubuntu. Be aware, however, that you can install only one version at a time. The packages are not listed in the default Ubuntu repositories. Therefore, we have to add the appropriate repositories to Ubuntu's source list, which you can find at /etc/apt/sources.list. You can open the file with any text editor. Make sure that you have super user rights, as you will need them to save your edits. One option is to use gedit, which is installed in Ubuntu by default. To edit the sources.list file, use the following command:

```
sudo gedit /etc/apt/sources.list
```

Downloading the example code
You can download the example code files for this book from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

Make sure that you add only one of the following package-source options to avoid conflicts due to incompatible packages. The specific lines that you have to add to the source list depend on your Ubuntu version:

- 1. The first option, which is also the default one, is to install the LR version. To install the QGIS LR release on Trusty, add the following lines to your file:
- deb http://qgis.org/debian trusty main
- deb-src http://qgis.org/debian trusty main

If necessary, replace trusty with precise, vivid, or wily to fit your system. For an updated list of supported Ubuntu versions, check out http://www.qgis.org/en/site/forusers/alldownloads.html#debian-ubuntu.

- 2. The second option is to install QGIS LTR by adding the following lines to your file:
- deb http://qgis.org/debian-ltr trusty main
- deb-src http://qgis.org/debian-ltr trusty main

3. The third option is to install QGIS DEV by adding these lines to your file:

```
deb http://qgis.org/debian-nightly trusty main
deb-src http://qgis.org/debian-nightly trusty main
```

> The preceding versions depend on other packages such as GDAL and proj4, which are available in the Ubuntu repositories. It is worth mentioning that these packages are often quite old.

4. The fourth option is to install QGIS LR with updated dependencies, which are provided by the ubuntugis repository. Add these lines to your file:

```
deb http://qgis.org/ubuntugis trusty main
deb-src http://qgis.org/ubuntugis trusty main
deb http://ppa.launchpad.net/ubuntugis/ubuntugis-unstable/
ubuntu trusty main
```

5. The fifth option is QGIS LTR with updated dependencies. Add these lines to your file:

```
deb http://qgis.org/ubuntugis-ltr trusty main
deb-src http://qgis.org/ubuntugis-ltr trusty main
deb http://ppa.launchpad.net/ubuntugis/ubuntugis-unstable/
ubuntu trusty main
```

6. The sixth option is the QGIS master with updated dependencies. Add these lines to your file:

```
deb http://qgis.org/ubuntugis-nightly trusty main
deb-src http://qgis.org/ubuntugis-nightly trusty main
deb http://ppa.launchpad.net/ubuntugis/ubuntugis-unstable/
ubuntu trusty main
```

> To follow the tutorials in this book, it is recommended that you install QGIS 2.14 LTR with updated dependencies (the fifth option).

After choosing the repository, we will add the qgis.org repository's public key to our apt keyring. This will avoid the warnings that you might otherwise get when installing from a non-default repository. Run the following command in the terminal:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key 3FF5FFCAD71472C4
```

> By the time this book goes to print, the key information might have changed. Refer to http://www.qgis.org/en/site/forusers/alldownloads.html#debian-ubuntu for the latest updates.

Finally, to install QGIS, run the following commands:

```
sudo apt-get update
```

```
sudo apt-get install qgis python-qgis qgis-plugin-grass
```

## Running QGIS for the first time

When you install QGIS, you will get two applications: QGIS Desktop and QGIS Browser. If you are familiar with ArcGIS, you can think of QGIS Browser as something similar to ArcCatalog. It is a small application used to preview spatial data and related metadata. For the remainder of this book, we will focus on QGIS Desktop.

By default, QGIS will use the operating system's default language. To follow the tutorials in this book, I advise you to change the language to English by going to Settings | Options | Locale.

On the first run, the way the toolbars are arranged can hide some buttons. To be able to work efficiently, I suggest that you rearrange the toolbars (for the sake of completeness, I have enabled all toolbars in Toolbars, which is in the View menu). I like to place some toolbars on the left and right screen borders to save vertical screen estate, especially on wide-screen displays.

Additionally, we will activate the file browser by navigating to View | Panels | Browser Panel. It will provide us with quick access to our spatial data. At the end, the QGIS window on your screen should look similar to the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_31_0.png)

Next, we will activate some must-have plugins by navigating to **Plugins | Manage and Install Plugins**. Plugins are activated by ticking the checkboxes beside their names. To begin with, I will recommend the following:

- **Coordinate Capture:** This plugin is useful for picking coordinates in the map
- **DB Manager:** This plugin helps you manage the SpatiaLite and PostGIS databases
- **fTools:** This plugin offers vector analysis and management tools
- **GdalTools:** This plugin offers raster analysis and management tools
- **Processing:** This plugin provides access to many useful raster and vector analysis tools, as well as a model builder for task automation

![](img/2e88bf5938bf48a409c0a534a8775b21_33_0.png)

Below the map, we find important information such as (from left to right) the current map Coordinate, map Scale, and the (currently inactive) project coordinate reference system (CRS), for example, EPSG:4326 in this screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_33_1.png)

Next, there are multiple toolbars to explore. If you arrange them as shown in the previous section, the top row contains the following toolbars:

- **File**: This toolbar contains the tools needed to **Create, Open, Save,** and **Print projects**

- **Map Navigation**: This toolbar contains the pan and zoom tools

- **Attributes**: These tools are used to *identify, select, open attribute tables, measure,* and so on, and looks like this:

![](img/2e88bf5938bf48a409c0a534a8775b21_33_2.png)

The second row contains the following toolbars:

- **Label**: These tools are used to add, configure, and modify labels

- **Plugins**: This currently only contains the **Python Console** tool, but will be filled in by additional Python plugins

- **Database**: Currently, this toolbar only contains DB Manager, but other database-related tools (for example, the **OfflineEditing** plugin, which allows us to edit offline and synchronize with databases) will appear here when they are installed
