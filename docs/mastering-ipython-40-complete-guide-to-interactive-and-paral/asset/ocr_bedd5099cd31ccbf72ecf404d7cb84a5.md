# Syntax

Type hints use the same syntax and apply to the same things as function annotations. The major extension is that they allow the annotation to be a class. The full rule set is complicated, but the following hints should cover the most common cases:

+   - Built-in classes
+   - User-defined classes
+   - Types in the `types` module
+   - Abstract base classes
+   - Special constructs, including `None`, `Any`, `Union`, `Tuple`, and `Callable`

A great deal of the PEP is devoted to special cases and complex types, including generics, covariance and contravariance, forward references, and stub files.

# Semantics

Like PEP 3107, there is no standard semantics associated with type hints. The goal instead was to provide a standard that third-party tools could work with. To the extent that type hints have a semantics, it is the one provided by the tool they are used with, not by the Python language itself. From the definition:

> "This PEP aims to provide a standard syntax for type annotations, opening up Python code to easier static analysis and refactoring, potential runtime type checking, and (perhaps, in some contexts) code generation utilizing type information."

An influential tool in the formation of the standard was mypy (http://mypy-lang.org/). The purpose of mypy is to provide a static type checker for Python. While the merits of static type checking in documentation and debugging can be debated, it is nice to have the option.

# Docstrings

A Python docstring is a string literal. It can occur as the first statement in a module or as the first statement following the definition of a function, method, or class. The docstring becomes the __doc__ special attribute of that thing. Its purpose is to provide a more free-form way to document part of a program.