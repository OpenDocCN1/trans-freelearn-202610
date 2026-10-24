
In *Figure 2.6*, we have imported the `sys` library (also known as system), and `sys` has an attribute named `version` that displays the current Python version along with some system information. For more information on sys and IDLE, please visit `https://docs.python.org/3/library/sys.html` and `https://docs.python.org/3/library/idle.html`, respectively.

The default Python setup installs most of the internal or default libraries (sys, urllib, os, math, and more), but if we want libraries that are not available, the system throws a `ModuleNotFoundError` error, as shown in *Figure 2.7*:

```
```text
>>> import urllib
>>> import requests
Traceback (most recent call last):
  File "<pyshell#3>", line 1, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'
>>> import pycurl
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    import pycurl
ModuleNotFoundError: No module named 'pycurl'
```
```

Figure 2.7: ModuleNotFoundError encountered when importing the requests library

Cases similar to *Figure 2.7* will require additional installation of such libraries, which can be handled using pip.

So far, Python has been installed, and we have verified the installation using the CLI and Python’s **Graphical User Interface (GUI)**, IDLE. To set up additional libraries, such as requests, we first have to take some additional steps, which we are going to cover in the next section.

## Creating a virtual environment

Before proceeding with the installation of the required libraries or additional libraries that are not available by default with Python, it is recommended to create a virtual environment for your project.

A virtual environment maintains a directory for a project and its required libraries so that no conflict between libraries and the system can happen. It normally provides a complete environment for targeted projects with their particular requirements and specifications. For more information on virtual environments, please visit `https://docs.python.org/3/tutorial/venv.html` or `https://docs.python-guide.org/dev/virtualenvs/`.

We will install all the required libraries and tools for our book in a virtual environment. So, let’s begin by creating the environment, and we will load it with the required libraries using pip by following these steps:

- 1. Create a directory first, for example, HOWScraping2E (this is where we want all of our code in the book to be placed) in the root drive (C:\HOWScraping2E>).
- 2. In HOWScraping2E, run the following command:
- 3. To activate the environment (secondEd) we have created, run the activate command using secondEd\Scripts, as shown in Figure 2.8:
- 4. To deactivate or close the environment, we can just issue the deactivate command, for example, (secondEd) C:\HOWScraping2E> deactivate. It is advisable to deactivate the environment at the end or before closing the system.

```
C:\HOWScraping2E>python -m venv secondEd
```

Here we have created a virtual environment named secondEd using venv.

This step also creates a directory named secondEd inside the HOWScraping2E directory, with a few more folders.

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_52_0.png)

Figure 2.8: Activating the virtual environment

The virtual environment has been successfully activated, as the secondEd environment is visible at the start of the command prompt, as shown in Figure 2.8.

Now that the location and environment are ready, we need to install the required libraries and tools using pip. The pip package management system is used to install and manage software packages written in Python. For more information, please visit https://pypi.org/project/pip/ and https://packaging.python.org/en/latest/tutorials/installing-packages/.

Let’s get some information on the version of pip that we are going to use, along with the path of pip in our virtual environment, as shown in Figure 2.9:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_52_1.png)

Figure 2.9: pip location and version, and Python version

> **Important note**
> 
> Normally, the `lib` folder contains libraries from the default Python installation in the system, and external libraries are found inside `site-packages`. As shown in Figure 2.9, we now have `lib` and `site-packages` inside our virtual environment, and in a project-specific or dedicated folder. Therefore, any library installations or updates will occur inside our targeted environment.
> 
> If you prefer using *Anaconda*, refer to `https://docs.anaconda.com/free/navigator/tutorials/manage-environments/` and `https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html`.

So far, our environment is ready and working. We have verified that pip exists in our system. In the next section, we will install the libraries we need.

## Installing libraries

In this section, we will install some libraries in our environment, such as `requests`, `jupyterlab`, and a few others as required.

Python notebooks are now in high demand – they provide easy visibility of code and a lot of control over input and output. In addition, the export feature is very useful to export code for presentations or explanations. Let’s install JupyterLab (`https://jupyter.org`) so we can use Python notebooks (`.ipynb` files). We can use pip install as shown in the following command to install JupyterLab:

```
```console
(secondEd) C:\HOWScraping2E> pip install jupyterlab
```
```

After successfully installing JupyterLab, let’s run it and check the availability of other libraries (many dependent libraries get installed during the setup of JupyterLab). To load `jupyterlab`, use the following command:

```
```console
(secondEd) C:\HOWScraping2E>jupyter-lab
```
```

This will open a new window pointing to `http://localhost:8888/lab` in your default browser, as shown in *Figure 2.10:*

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_54_0.png)

Figure 2.10: JupyterLab (the default window in Google Chrome)

> **Important note**
> 
> For more information on using Python notebooks and JupyterLab, please visit `https://docs.jupyter.org/en/latest/`. In addition, you can create a notebook and check whether you need to install other libraries too – if they are missing, we can install them from the notebook itself (using `!pip install requests`).

Our decision to install and use JupyterLab is quite convenient because major libraries such as requests are automatically installed and available to us. As shown in *Figure 2.7*, the requests library is not available with the default Python installation. Let’s verify that our other required libraries are working fine; use them or import them in a notebook, as shown in *Figure 2.11* (the code is available in the `https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter02/test_install_libraries.ipynb` file):

```
[2]: import requests
```

```
[3]: requests.__version__
```

```
[3]: '2.28.1'
```

```
[4]: import urllib
```

```
[5]: list(dir(urllib))
```

```
[5]: ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', 'error', 'parse', 'request', 'response']
```

Figure 2.11: Verifying the requests and urllib libraries

If you do not wish to use JupyterLab, you can set up the libraries from the terminal using the following code:

```
(secondEd) C:\HOWScraping2E> pip install requests
```

Now, our environment is set up and the required libraries are in our system. We will use these libraries to communicate with URLs (send requests to the web and collect responses) in the next section.

## Loading URLs

Loading URLs generally involves HTTP communication, such as sending requests to the server and receiving responses from the server. The upcoming URL handling and operations and Implementing HTTP methods sections in this chapter will explore these topics in more detail. Data extraction activities will be covered in Chapter 3.

> **Important note**
> Before loading URLs using Python, it’s also advisable to verify that the URLs are working properly and contain the data that we are looking for. This basic feasibility lookup will save many resources while running and managing our projects.
