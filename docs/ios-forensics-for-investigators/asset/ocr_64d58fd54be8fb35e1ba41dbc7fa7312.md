As you can see from the preceding figure, for each record, Physical Analyzer will specify the source file from which the record was parsed. Keep in mind that just because a location appears in the results, it doesn't necessarily mean that the device actually visited that location, as this tool will also display harvested data.

# Analyzing location data with Apollo

Apollo, which we introduced in the previous chapter, is a Python script that correlates multiple sources of data into a unified **timeline**, simplifying the examiner's job of finding out what exactly was happening on the device. The tool consists of dozens of highly configurable modules that each query a specific iOS database to extract data and events. Apollo supports both routined location data and harvested data.

We'll now go over the steps required to download the tool, run it, and export location data into a KMZ file that can be imported into a map provider service:

+   1. To download Apollo, head over to its GitHub repository located at https://github.com/mac4n6/APOLLO and download the latest version as a ZIP archive or by cloning the repository by running the git clone command.
+   2. Next, run the tool by specifying the export format, the location of the module's directory, and the location of the data that should be analyzed, such as the folder that contains the SQLite databases or the full filesystem extraction. For example, running python3 apollo.py extract -o sql -p apple -v 14 -k modules/ fs-extraction/ will run Apollo in SQL mode, analyzing the files in the fs-extraction folder and running all modules found in the modules folder. Make sure you use the -k option to output location files.
+   3. When the tool ends the process, the results will be stored in a file called apollo.db and in several KMZ files.
+   4. You can then proceed to import the KMZ files using your tool of choice, such as **Google Earth** or **Google Maps**.