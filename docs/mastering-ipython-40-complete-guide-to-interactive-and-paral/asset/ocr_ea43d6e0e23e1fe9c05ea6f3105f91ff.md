# Documenting source files

Unfortunately, unlike Javadoc, Docutils cannot take a .py file full of Python code and automatically extract the docstrings from it in order to generate documentation. There is a tool called Epydoc that will do this, but the project has been inactive since 2009. A team could write a script to attempt to extract just the docstrings, but there is a better solution. It will be outlined in the next section.

# Sphinx

Sphinx is a documentation generation tool. It began as a way to produce Python documentation, but it is branching out to other languages, notably C/C++. It can produce output in several formats, including HTML, LaTeX, ePub, man pages, and plain text, among others. It accepts reStructuredText and builds upon the Docutils suite, adding a few pieces of markup along the way.

# Installation and startup

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