
The entire views.py file is shown in the following screenshot:

![](img/4840a43efd335abd10b93708225692d6_73_0.png)

Figure 1.40 – views.py with the name variable set in the render context

- 7. Refresh your browser again, and you’ll see it now says Hello, world!.

![](img/4840a43efd335abd10b93708225692d6_73_1.png)

Figure 1.41 – A template rendered with a variable

In this exercise, we combined the template we created in the previous exercise with the render function to render an HTML page, with the name variable that was passed to it inside a context dictionary.

In the next section, we’ll look at some ways you can debug your code and handle errors.

## Debugging and dealing with errors

When programming, unless you’re the perfect programmer who never makes mistakes, you’ll probably have to deal with errors or debug your code at some point. When there is an error in your program, there are usually two ways to tell – either your code will raise an exception, or you will get unexpected output or results when viewing the page. You will probably see exceptions more often, as there are many accidental ways to cause them. If your code is generating unexpected output but not raising any exceptions, you will probably want to use the PyCharm debugger to find out why.

We’ll start with an overview of some Python exceptions and how to handle them, along with an exercise that demonstrates how Django shows exceptions. Then, we’ll look at running Django inside the PyCharm debugger so that you can peek inside your program while it executes.

## Exceptions

If you have worked with Python or other programming languages before, you have probably come across exceptions. If not, here’s a quick introduction. Exceptions are *raised* (or *thrown* in other languages) when an error occurs. The execution of a program stops at that point in the code, and the exception travels back up the function call chain until it is caught. If it is not caught, then the program will crash, sometimes with an error message describing the exception and where it occurred. There are exceptions that are raised by Python itself, and your code can raise exceptions to quickly stop execution at any point. Some common exceptions that you might see when programming Python are as follows:

- **IndentationError**: Python will raise this if your code is not correctly indented or has mixed tabs and spaces
- **SyntaxError**: Python raises this error if your code has invalid syntax:
```
>>> a === 1
  File "<stdin>", line 1
    a === 1
        ^
SyntaxError: invalid syntax
```
- **ImportError**: This is raised when an import fails – for example, if trying to import from a file that does not exist or trying to import a name that is not set in a file:
```
>>> import missing_file
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named missing_file
```

```
>>> a === 1
  File "<stdin>", line 1
    a === 1
        ^
SyntaxError: invalid syntax
```

```
>>> import missing_file
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named missing_file
```

- NameError: This is raised when trying to access a variable that has not yet been set:
```
>>> a = b + 5
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'b' is not defined
```
- KeyError: This is raised when accessing a key that is not set in a dict (or a dict-like object):
```
>>> d = {'a': 1}
>>> d['b']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'b'
```
- IndexError: This is raised when accessing an index outside the length of a list:
```
>>> l = ['a', 'b']
>>> l[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```
- TypeError: This is raised when trying to perform an operation on an object that does not support it, or when using two objects of the wrong type – for example, trying to add a string to an int:
```
>>> 1 + '1'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

Django also raises its own custom exceptions, and you will be introduced to them throughout the book.

When running the Django development server with DEBUG = True in your settings.py file, Django will automatically capture exceptions that occur in your code (instead of crashing). It will then generate an HTTP response, showing you a stack trace and other information to help you debug the problem. When running in production, DEBUG should be set to False. Django will then return a standard internal server error page, without any sensitive information. You also have the option to display a custom error page.

## Exercise 1.08 – generating and viewing exceptions

Let’s create a simple exception in our view so that you are familiar with how Django displays them. In this case, we’ll try to use a variable that doesn’t exist, which will raise NameError:

- 1. In PyCharm, open views.py. In the index view function, change the context being sent to the render function so that it’s using a variable that doesn’t exist. We’ll try to send invalid_name in the context dictionary, instead of name. Don’t change the context dictionary key, just its value:
```
return render(request, "base.html", {"name": invalid_name})
```
- 2. Go back to your browser and refresh the page. You should see a screen like Figure 1.42:

```
return render(request, "base.html", {"name": invalid_name})
```

![](img/4840a43efd335abd10b93708225692d6_76_0.png)

Figure 1.42 – A Django exception screen

# Figure 1.45 – A breakpoint on line 5

4. Go back to your browser and refresh the page. Your browser will not display any content; instead, it will just continue to try to load the page. Depending on your operating system, PyCharm should become active again; if not, bring it to the foreground. You should see that line 5 is highlighted, and at the bottom of the window, the debugger is shown. The stack frames (the chain of functions that were called to get to the current line) are on the left, and the current variables of the function are on the right.

![](img/4840a43efd335abd10b93708225692d6_79_0.png)

# Figure 1.46 – The debugger paused with the current line (5) highlighted

There is currently one variable in scope, request. If you click the toggle triangle to the left of its name, you can show or hide the attributes it has set.

![](img/4840a43efd335abd10b93708225692d6_80_0.png)

Figure 1.47 – The attributes of the request variable

For example, if you scroll down through the list of attributes, you can see that method is GET and path is /.

![](img/4840a43efd335abd10b93708225692d6_80_1.png)

Figure 1.48 – The actions bar

5. The actions bar, shown in Figure 1.48, is above the stack frames and variables. Its buttons (from left to right) are as follows:

- Step Over: This executes the current line of code and continues to the next line.
- Step Into: This steps into the current line. For example, if the line contained a function, it would continue with the debugger inside this function.

- **Step into My Code**: This steps into the line being executed but continues until it finds the code you have written. For example, if you’re stepping into third-party library code that later calls your code, it will not show you the third-party code, instead continuing through until it returns to the code that you have written.
- **Step Out**: This returns from the current code to the function or the method that called it. It is the opposite of the **Step In** action.
- **Run to Cursor**: If you have a line of code further along from where you currently are that you want to execute to, without having to click Step Over for all the lines in between, click to put your cursor on that line. Then, click **Run to Cursor**, and the execution will continue until that line.

Note that not all buttons are useful all the time. For example, it can be easy to step out of your view and end up confusing Django library code.

- 6. Click the **Step Over** button once to execute line 5.
- 7. You can see that the name variable has been added to the list of variables in the debugger view, and its value is world.

![](img/4840a43efd335abd10b93708225692d6_81_0.png)

Figure 1.49 – The new variable name is now in scope, with the world value

- 8. We are now at the end of our index view function, and if we were to step over this line of code, it would jump to Django library code, which we don’t want to see. To continue executing and to send the response back to your browser, click the **Resume Program** button on the left of the window (Figure 1.50). You should see that your browser has now loaded the page again:

![](img/4840a43efd335abd10b93708225692d6_81_1.png)

Figure 1.50 – Actions to control execution – the green play icon is the Resume Program button
