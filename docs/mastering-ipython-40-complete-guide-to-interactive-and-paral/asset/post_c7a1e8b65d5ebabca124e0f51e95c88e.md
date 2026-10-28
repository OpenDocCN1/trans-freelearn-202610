
## Documenting source files

Unfortunately, unlike Javadoc, Docutils cannot take a .py file full of Python code and automatically extract the docstrings from it in order to generate documentation. There is a tool called Epydoc that will do this, but the project has been inactive since 2009. A team could write a script to attempt to extract just the docstrings, but there is a better solution. It will be outlined in the next section.

## Sphinx

Sphinx is a documentation generation tool. It began as a way to produce Python documentation, but it is branching out to other languages, notably C/C++. It can produce output in several formats, including HTML, LaTeX, ePub, man pages, and plain text, among others. It accepts reStructuredText and builds upon the Docutils suite, adding a few pieces of markup along the way.

## Installation and startup

Sphinx can be installed using `pip`:

```
pip install sphinx
```

Installation will add several scripts to the path. The first one we will examine is named `sphinx-quickstart`. It will set up a prompt the user for information and then a source directory, a configuration file, and some skeleton files:

```
bash-4.1$ sphinx-quickstart
Welcome to the Sphinx 1.3.6 quickstart utility.

Please enter values for the following settings (just press Enter to accept a default value, if one is given in brackets).

Enter the root path for documentation.
> Root path for the documentation [.] : docs

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
```

## Documentation

This results in the following page:

![](img/8628b86fcc4f22d859c0104c2ec1d15f_324_0.png)

This is missing documentation for the `chinese_remainder` method, so that can be added to `index.rst` using the automethod directive:

```rst
.. toctree::
   :maxdepth: 2

.. automodule:: crt1
```

While flexible, this can be tedious and error-prone. One solution is to use the `:members:` attribute, like this:

```
    .. toctree::
       :maxdepth: 2

    .. automodule:: crt1
       :members:
```

Another is to use the `sphinx-apidoc` tool. The purpose of `sphinx-apidoc` is to extract the docstrings from the source files and produce `.rst` files from them. Consider this example:

```
bash-4.1$ sphinx-apidoc -o docs .
```

It will create a set of `.rst` files:

```
Creating file docs/code1.rst.
Creating file docs/code2.rst.
Creating file docs/crt1.rst.
Creating file docs/hail1.rst.
Creating file docs/modules.rst.
```

The files themselves are not much to look at, but they can be fed into Sphinx or other documentation tools that are expecting reStructuredText for further processing. This is especially handy when using tools that cannot extract docstrings on their own.

## Summary

While inline comments and type notations are useful for developers who will be working on the code base, external parties need more extensive documentation, and need it in a more attractive format than reading the source (this is especially true when they do not have access to the source). This need for documentation must be balanced against the understandable reluctance of developers to spend a great deal of time and energy producing anything but code.

Python's solution is to employ a lightweight markup language, reStructuredText, and a toolchain (Docutils, Sphinx) to transform reStructuredText into a more aesthetically attractive format (for example, HTML or LaTeX).

Chapter 3, Stepping Up to IPython for Parallel Computing, provides some details on how splitting out the kernel and REPL protocol enabled a parallel architecture to emerge.

This chapter will provide some examples of the power of the notebook, and how it relates to the other components. The move from the IPython notebook to the Jupyter project was probably the biggest of the reorganizations, and has been called The Big Split. For an in-depth look at the reasons for The Big Split, and details regarding its effect on various package structures, see https://blog.jupyter.org/2015/04/15/the-big-split/.

## Installation and startup

Jupyter can be installed using pip:

```
pip install jupyter
```

It can be started like this:

```
(my_rpy2_zone)-bash-4.1$ ipython notebook
[I 10:38:27.193 NotebookApp] Serving notebooks from local directory: /nfs/02/wit0096
[I 10:38:27.193 NotebookApp] 0 active kernels
[I 10:38:27.193 NotebookApp] The Jupyter Notebook is running at: http://localhost:8888/
[I 10:38:27.193 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
/usr/bin/xdg-open: line 402: htmlview: command not found
console.error:
  [CustomizableUI]
  Custom widget with id loop-button does not return a valid node
console.error:
  [CustomizableUI]
  Custom widget with id loop-button does not return a valid node
1457711201438  addons.update-checker  WARN  Update manifest for {972ce4c6-7e08-4474-a285-3208198ce6fd} did not contain an updates property
```

Note that this has started an HTML server (called the Jupyter Notebook App) on port 8888. It may also (depending on your platform) start a browser pointed at http://localhost:8888. If not, manually start a browser and point it there. Jupyter can only access files in the directory it was started in (and its subdirectories), so take care to start it where it can access your notebook files.

## Creating a notebook

Of course, no one writes a notebook in JSON – it is just used as a text format for persistence and interchange. The Dashboard can be used to create notebooks. In the upper-right section of the interface is a dropdown box that provides options to create new notebooks:

![](img/8628b86fcc4f22d859c0104c2ec1d15f_336_0.png)

## Using matplotlib

The key to using matplotlib in a notebook is the `%matplotlib inline` magic. It should be the first line in the cell. For example:

![](img/8628b86fcc4f22d859c0104c2ec1d15f_344_0.png)

The first cell (which prints **Graphics are fun**) is Markdown; the second cell creates the plot shown.

## Using R

The important thing when using R is that all the work should be done in R (or at least the part that actually displays the plot). If not, the plot will open in a separate window, rather than in the notebook. The %%R cell magic is the right tool for the job. In particular, the plot should be printed out, rather than using the plot( ) function. For example:

![](img/8628b86fcc4f22d859c0104c2ec1d15f_346_0.png)

IPython is not such a system. In 2001, Fernando Perez was a graduate student at the University of Colorado in Boulder. He was not a computer science major, he was a physics graduate student who needed to program and wanted a better tool. As a physicist, his idea of "better" was different from what a computer scientist might have envisioned. In particular, the formative influences on the young IPython was tools designed primarily for data analysis and presentation: Mathematica (http://www.wolfram.com/mathematica/) and Sage (http://www.sagemath.org/).

Perez has stated that IPython is "meant to be a highly configurable tool for building problem-specific interactive environments," which provides a key to its history and future directions.

Initial efforts came at the problem from two directions. On one hand, there were a couple of Python extensions that already existed – IPP (by Janko Hauser) and LazyPython (by Nathan Gray). On the other hand, Perez had his own ideas about what should be implemented. In the end, IPython was born through a merger of code from IPP, LazyPython, and Perez's own efforts.

At this point development slowed down. A few stabs at making a notebook were made. Some consideration was given to merging with Sage, but in the end the projects went their own ways. In 2009, the first full-time developer was funded. His first task was a refactoring of the entire codebase, which was at that point still a mash of IPP, LazyPython, and Perez's original code. No new functionality was added, but development could move forward more easily on the newly clean code.

2010 was an important year. The first big development was the move to an architecture based on ZeroMQ. The adoption of ZeroMQ allowed for two important architectural advances: separation between the frontend and the interpreter, and support for parallelism.

Separating the frontend and backend was huge on several fronts. Perhaps most importantly, it meant that implementing a notebook was simple(r): the Notebook became just another interface to the Python kernel on the back end. On the flip side, the command-line interface was free to connect to a kernel that supported any language, not just Python. IPython was moving from "a better Python shell" to "a multi-interface, multi-language platform." Work proceeded quickly at this point: the remainder of 2010 saw the development of a command-line interface, a Qt console, integration into Python Tools for Visual Studio, and a prototype web notebook. 2011 saw the prototype turned into a fully-working version, and IPython 0.12 was released.
