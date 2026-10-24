+   4. Change the file paths for the result file in line 4 depending on your operating system.
+   5. Save the script and run it. Don't forget to select the vector layer in the QGIS layer tree before running the script.

# How it works...

In line 1, we imported the csv module from the standard Python library. This module provides a convenient way to read and write comma-separated files. In line 3, we obtained a reference to the currently selected layer, which will be used later to access layer features.

In line 3, an output file opened. Note that here we use the with statement so that later there is no need to close the file explicitly, context manager will do this work for us. In line 5, we set up the so-called writer—an object that will write data to the CSV file using specified format settings.

In line 6, we started iterating over features of the active layer. For each feature, we extracted its geometry and converted it into a Well-Known Text (WKT) format (line 7). We then wrote this text representation of the feature geometry with some attributes to the output file (line 8).

It is necessary to mention that our script is very simple and will work only with attributes that have ASCII encoding. To handle non-Latin characters, it is necessary to convert the output data to the unicode before writing it to file.

# There's more...

Using the Python console, you also can invoke Processing algorithms to create complex scripts for automated analysis and/or data preparation.

To make the Python console even more useful, take a look at the Script Runner plugin. Detailed information about this plugin with some usage examples can be found at http://spatialgalaxy.net/2012/01/29/script-runner-a-plugin-to-run-python-scripts-in-qgis/.

# See also

+   If you are new to Python and QGIS API, don't forget to look at the following documentation:
+     Official Python documentation and tutorial can be found at https://docs.python.org/2/
+     QGIS API Documentation can be found at http://qgis.org/api/2.8/
+     PyQGIS Developer Cookbook can be found at http://docs.qgis.org/2.8/en/docs/pyqgis_developer_cookbook/