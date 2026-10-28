```
retval = quad("myFunc", 0, pi)
disp("hello")
endfunction

function y = myFunc(x)
    y = sin(x) * sqrt(abs(cos(x)))
endfunction
```

We can use these IPython commands to run it:

```
In [1]: from oct2py import octave

In [2]: octave.addpath("/nfs/02/wit0096/Packt/chap06/")

In [3]: octave.myIntegrate( )
Out[3]: 1.3333333333333326
```

This yields an easy integration of a complicated function.

# Hy

Sometimes, nothing less than a functional programming language will do. This is especially true in machine learning and artificial intelligence, where Lisp and Scheme have a long tradition. Hy is a dialect of Lisp that translates expressions into Python's abstract syntax tree format.

An abstract syntax tree is a high-level representation of a program's structure, independent of the source code. The Python parser turns all Python programs into abstract syntax trees, after which the interpreter performs further operations on the tree on the path to creating executable bytecode. Because the Hy frontend accepts Hy code as input and produces as output an abstract syntax tree compatible with Python's interpreter, Hy code runs in the same interpreter as your Python code.

In practical terms, Hy programs can call any Python libraries and Python code can invoke Hy code.

It is available under the MIT (Expat) license from its home page at `http://docs.hylang.org/en/latest/`.

> > "Hy is a wonderful dialect of Lisp that's embedded in Python.
> >
> > Since Hy transforms its Lisp code into the Python Abstract Syntax Tree, you have the whole beautiful world of Python at your fingertips, in Lisp form!"