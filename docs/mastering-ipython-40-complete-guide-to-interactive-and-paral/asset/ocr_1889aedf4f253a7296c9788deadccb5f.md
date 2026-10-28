```
addTypeEqualityFunc(self, typeobj, function)

Add a type specific assertEqual style function to compare a type.

This method is for use by TestCase subclasses that need to register
their own type equality functions to provide nicer error messages.

Args:
    typeobj: The data type to call this function on when both values
        are of the same type in assertEqual().
    function: The callable taking two arguments and an optional
        msg= argument that raises self.failureException with a
        useful error message when the two arguments are not equal.

assertAlmostEqual(self, first, second, places=None, msg=None, delta=None)
```

Note how the documentation for the `unittest.TestCase` class is included without additional effort.

# Recommended elements

PEP 0257 documents the conventions associated with docstrings. From the standard (https://www.python.org/dev/peps/pep-0257/):

> > "The aim of this PEP is to standardize the high-level structure of docstrings: what they should contain, and how to say it (without touching on any markup syntax within docstrings). The PEP contains conventions, not laws or syntax."

While there is no requirement that developers should follow these guidelines, third-party tools (such as Docutils and Sphinx) may rely upon them to extract features from docstrings.