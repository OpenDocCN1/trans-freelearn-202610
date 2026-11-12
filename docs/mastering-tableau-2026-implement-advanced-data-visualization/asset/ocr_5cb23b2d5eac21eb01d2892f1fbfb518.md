This was the R part of this chapter. I hope you enjoyed it and that it has given you the inspiration to produce your own ideas to extend the functionality we have shared so far. We will now continue with the other very well-known programming language: Python. Tableau provides a similar integration layer for Python via TabPy.

# # Python installation and integration

Python is an interpreted programming language and is very well known for its readability. The first release was in 1991, so quite some time ago (longer than most people would guess), and it was developed by Guido van Rossum. TabPy is an external service that will allow you to connect Python and Tableau—similar to Rserve. By using TabPy, you will be able to parse fields from your Tableau dashboard to Python, execute a calculation, and send back the result as a newly calculated field to Tableau. Or you can also call functions that you implemented in Python, again in a calculated field. A more extensive article on TabPy can be found here: https://www.datacamp.com/tutorial/how-to-run-python-in-tableau-with-tabpy

Installing Python is typically not difficult, but it does involve more than simply double-clicking on an executable. To successfully connect Tableau with Python, you might have to install some libraries and execute commands on the command line. The following paragraphs will guide you through this process.

# Python 3.11 Support

TabPy now supports Python 3.11, bringing performance improvements and access to the latest Python features. The documentation has been updated accordingly. If you're starting fresh, I recommend installing Python 3.11 or later to ensure compatibility with the latest libraries and security updates.

The TabPy 2.0 release includes several important enhancements:

+   - Python 3.11 support – Full compatibility with Python 3.11, ensuring access to the latest language features and security updates.
+   - Updated dependencies – All underlying libraries have been updated to their latest versions, improving performance and security.
+   - Streamlined deployment – The model deployment process has been optimized, making it easier to deploy and manage custom models.
+   - Better error handling – More informative error messages when something goes wrong with your Python scripts, helping you debug faster.

These improvements make the Tableau-Python integration more robust and easier to work with than ever before.