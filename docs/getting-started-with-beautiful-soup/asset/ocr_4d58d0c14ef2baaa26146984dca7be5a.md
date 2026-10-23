# Installing Beautiful Soup using setup.py

We can install Python packages using the `setup.py` script that comes with every Python package downloaded from the Python package index website: `https://pypi.python.org/`. The following steps are used to install the Beautiful Soup using `setup.py`:

+   1. Download the latest tarball from https://pypi.python.org/packages/source/b/beautifulsoup4/.
+   2. Unzip it to a folder (for example, `BeautifulSoup`).
+   3. Open up the command-line prompt and navigate to the folder where you have unzipped the folder as follows:
+      `cd BeautifulSoup`
+      `python setup.py install.`
+   4. The `python setup.py install` line will install Beautiful Soup in our system.

> We are not done with the list of possible options to use Beautiful Soup. We can use Beautiful Soup in our applications even if all of the options outlined until now fail.

# Using Beautiful Soup without installation

The installation processes that we have discussed till now normally copy the module contents to a chosen installation directory. This varies from operating system to operating system and the path is normally `/usr/local/lib/pythonX.Y/site-packages` in Linux operating systems such as Debian and `C:\PythonXY\Lib\site-packages` in Windows (where X and Y represent the corresponding versions, such as Python 2.7). When we use import statements in the Python interpreter or as a part of a Python script, normally what the Python interpreter does is look in the predefined Python `Path` variable and look for the module in those directories. So, installing actually means copying the module contents into the predefined directory or copying this to some other location and adding the location into the Python path. The following method of using Beautiful Soup without going through the installation can be used in any operating system, such as Windows, Linux, or Mac OS X:

+   1. Download the latest version of Beautiful Soup package from https://pypi.python.org/packages/source/b/beautifulsoup4/.
+   2. Unzip the package.
+   3. Copy the `bs4` directory into the directory where we want to place all our Python Beautiful Soup scripts.