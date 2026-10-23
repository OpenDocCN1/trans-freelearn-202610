After we perform all the preceding steps, we are good to use Beautiful Soup. In order to import Beautiful Soup in this case, either we need to open the terminal in the directory where the bs4 directory exists or add this directory to the Python Path variable; otherwise, we will get the module not found error. This extra step is required because the method is specific to a project where the bs4 directory is included. But in the case of installing methods, as we have seen previously, Beautiful Soup will be available globally and can be used in any of the projects, and so the additional steps are not required.

# Verifying the installation

To verify the installation, perform the following steps:

+   1. Open up the Python interpreter in a terminal by using the following command:
+      `python`
+   2. Now, we can issue a simple import statement to see whether we have successfully installed Beautiful Soup or not by using the following command:
+      `from bs4 import BeautifulSoup`

If we did not install Beautiful Soup and instead copied the bs4 directory in the workspace, we have to change to the directory where we have placed the bs4 directory before using the preceding commands.

# Quick reference

The following table is an overview of commands and their implications:

| Command | Description |
|---|---|
| `sudo apt-get install python-bs4` | This command is used for installing Python using a package manger in Linux. |
| `sudo pip install beautifulsoup4` | This command is used for installing Python using pip. |
| `sudo easy_install beautifulsoup4` | This command is used for installing Python using easy_install. |
| `python setup.py install` | This command is used for installing Python using setup.py. |
| `from bs4 import BeautifulSoup` | This command is used for verifying installation. |