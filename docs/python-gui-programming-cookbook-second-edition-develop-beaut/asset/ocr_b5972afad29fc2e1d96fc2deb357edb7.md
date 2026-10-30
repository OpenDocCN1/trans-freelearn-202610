# How to use StringVar()

There are built-in programming types in tkinter that differ slightly from the Python types we are used to programming with. StringVar() is one such tkinter type. This recipe will show you how to use the StringVar() type.

# Getting ready

You will learn how to save data from the tkinter GUI into variables so we can use that data. We can set and get their values, which is very similar to the Java getter/setter methods.

Here are some of the available types of coding in tkinter:

| strVar = StringVar() | # Holds a string; the default value is an empty string "" |
| intVar = IntVar() | # Holds an integer; the default value is 0 |
| dbVar = DoubleVar() | # Holds a float; the default value is 0.0 |
| blVar = BooleanVar() | # Holds a Boolean, it returns 0 for False and 1 for True |

![](bbox=[0.06, 0.55, 0.15, 0.64])

Different languages call numbers with decimal points as floats or doubles. Tkinter calls them DoubleVar, what is known in Python as float datatype. Depending on the level of precision, float and double data can be different. Here, we are translating tkinter DoubleVar into what Python turns into a Python float type.