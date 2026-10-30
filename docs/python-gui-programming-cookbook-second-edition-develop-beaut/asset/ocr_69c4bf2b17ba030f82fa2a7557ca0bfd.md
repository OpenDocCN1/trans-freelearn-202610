# A little note about classes and types:

+   - In Python, every variable always has a type. We cannot create a variable that does not have a type. Yet, in Python, we do not have to declare the type beforehand, as we have to do in the C programming language.
+   - Python is smart enough to infer the type. C#, at the time of writing this book, also has this capability. Using Python, we can create our own classes using the `class` keyword instead of the `def` keyword.
+   - In order to assign the class to a variable, we first have to create an instance of our class. We create the instance and assign this instance to our variable, for example:

```
class AClass(object):
    print('Hello from AClass')
class_instance = AClass()
```

Now, the variable, `class_instance`, is of the AClass type. If this sounds confusing, do not worry. We will cover OOP in the coming chapters.

In line 15, we use the instance variable (`win`) of the class to give our window a title via the `title` property. In line 20, we start the window's event loop by calling the `mainloop` method on the class instance, `win`. Up to this point in our code, we created an instance and set one property, but the GUI will not be displayed until we start the main event loop.

+   - An event loop is a mechanism that makes our GUI work. We can think of it as an endless loop where our GUI is waiting for events to be sent to it. A button click creates an event within our GUI, or our GUI being resized also creates an event.
+   - We can write all of our GUI code in advance and nothing will be displayed on the user's screen until we call this endless loop (`win.mainloop()`) in the preceding code. The event loop ends when the user clicks the red X button or a widget that we have programmed to end our GUI. When the event loop ends, our GUI also ends.