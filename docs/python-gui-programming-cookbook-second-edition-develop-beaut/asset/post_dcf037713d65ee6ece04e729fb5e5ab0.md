
# Displaying widget text in different languages

The easiest way to internationalize text strings in Python is by moving them into a separate Python module and then selecting the language to be displayed in our GUI by passing in a parameter to this module.

While this approach is not highly recommended, according to online search results, depending on the specific requirements of the application you are developing, this approach might still be the most pragmatic and fastest to implement.

## Getting ready

We will reuse the Python GUI we created earlier. We have commented out one line of Python code that creates the MySQL tab because we do not talk to a MySQL database in this chapter.

## How to do it...

In this recipe, we will start the I18N of our GUI by changing the Windows title from English to another language.

As the name GUI is the same in other languages, we will first expand the name which enables us to see the visual effects of our changes.

The following was our previous line of code:

```
self.win.title("Python GUI")
```

Let's change this to the following:

```
self.win.title("Python Graphical User Interface")
```

The preceding code change results in the following title for our GUI program:

```
GUI_Refactored.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_278_0.png)

> In this chapter, we will use English and German to exemplify the principle of internationalizing our Python GUI.

Hardcoding strings into code is never too good an idea, so the first thing we can do to improve our code is to separate all the strings that are visible in our GUI into a Python module of their own. This is the beginning of internationalizing the visible aspects of our GUI.

> While we are into I18N, we will do this very positive refactoring and language translation, all in one step.

Let's create a new Python module and name it `LanguageResources.py`. Let's next move the English string of our GUI title into this module and then import this module into our GUI code.

> We are separating the GUI from the languages it displays, which is an OOP design principle.

Our new Python module, containing internationalized strings, now looks as follows:

```
class I18N():
    '''Internationalization'''
    def __init__(self, language):
        if language == 'en': self.resourceLanguageEnglish()
        elif language == 'de': self.resourceLanguageGerman()
        else: raise NotImplementedError('Unsupported language.')

    def resourceLanguageEnglish(self):
```

```
self.title = "Python Graphical User Interface"

def resourceLanguageGerman(self):
    self.title = 'Python Grafische Benutzeroberflaeche'
```
```

We import this new Python module into our main Python GUI code, and then use it:

```
from Ch08_Code.LanguageResources import I18N
class OOP():
    def __init__(self):
        self.win = tk.Tk()                  # Create instance
        self.i18n = I18N('de')              # Select language
        self.win.title(self.i18n.title)     # Add a title
```
```

Depending on which language we pass into the I18N class, our GUI will be displayed in that language.

Running the above code, we now get the following internationalized result:

GUI.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_279_0.png)

## How it works...

We break out the hardcoded strings that are part of our GUI into their own separate modules. We do this by creating a class, and within the class's `__init__()` method, we select which language our GUI will display, depending on the passed-in language argument.

This works.

We can further modularize our code by separating the internationalized strings into separate files, potentially in XML or another format. We could also read them from a MySQL database.

This is a Separation of Concerns coding approach, which is at the heart of OOP programming.

## Changing the entire GUI language, all at once

In this recipe, we will change all of the GUI display names, all at once, by refactoring all the previously hardcoded English strings into a separate Python module and then internationalizing those strings.

This recipe shows that it is a good design principle to avoid hardcoding any strings that our GUI displays but to separate the GUI code from the text that the GUI displays.

Designing our GUI in a modular way makes internationalizing it much easier.

## Getting ready

We will continue to use the GUI from the previous recipe. In that recipe, we had already internationalized the title of the GUI.

## How to do it...

In order to internationalize the text displayed in all of our GUI widgets, we have to move all hardcoded strings into a separate Python module, and this is what we'll do next.

Previously, strings of words that our GUI displayed were scattered all over our Python code.

Here is what our GUI looked like without I18N:

```
GUI_Refactored.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_281_0.png)

Every single string of every widget, including the title of our GUI, the tab control names, and so on, were all hardcoded and intermixed with the code that creates the GUI.

> It is a good idea to think about how we can best internationalize our GUI at the design phase of our GUI software development process.

The following is an excerpt of what our code looks like:

```
WIDGET_LABEL = ' Widgets Frame '
class OOP():
    def __init__(self):
        self.win = tk.Tk()              # Create instance
        self.win.title("Python GUI")    # Add a title
```

```
# Radiobutton callback function
def radCall(self):
    radSel=self.radVar.get()
    if    radSel == 0: self.monty2.configure(text='Blue')
    elif radSel == 1: self.monty2.configure(text='Gold')
    elif radSel == 2: self.monty2.configure(text='Red')
```
```

In this recipe, we are internationalizing all strings displayed in our GUI widgets. We are not internationalizing the text *entered* into our GUI, because this depends on the local settings on your PC.

The following is the code for the english internationalized strings:

```
class I18N():
'''Internationalization'''
    def __init__(self, language):
        if    language == 'en': self.resourceLanguageEnglish()
        elif language == 'de': self.resourceLanguageGerman()
        else: raise NotImplementedError('Unsupported language.')

    def resourceLanguageEnglish(self):
        self.title = "Python Graphical User Interface"

        self.file = "File"
        self.new  = "New"
        self.exit = "Exit"
        self.help = "Help"
        self.about = "About"

        self.WIDGET_LABEL = ' Widgets Frame '

        self.disabled  = "Disabled"
        self.unChecked = "UnChecked"
        self.toggle    = "Toggle"

        # Radiobutton list
        self.colors   = ["Blue", "Gold", "Red"]
        self.colorsIn = ["in Blue", "in Gold", "in Red"]

        self.labelsFrame  = ' Labels within a Frame '
        self.chooseNumber = "Choose a number:"
        self.label2       = "Label 2"

        self.mgrFiles = ' Manage Files '

        self.browseTo = "Browse to File..."
        self.copyTo   = "Copy File To :"
```
```

In our Python GUI module, all previously hardcoded strings are now replaced by an instance of our new I18N class, which resides in the LanguageResources.py module.

Here is an example from our refactored GUI.py module:

```
from Ch08_Code.LanguageResources import I18N
class OOP():
    def __init__(self):
        self.win = tk.Tk()                  # Create instance
        self.i18n = I18N('de')              # Select language
        self.win.title(self.i18n.title) # Add a title

    # Radiobutton callback function
    def radCall(self):
        radSel = self.radVar.get()
        if  radSel == 0: self.widgetFrame.configure(text=
                        self.i18n.WIDGET_LABEL + self.i18n.colorsIn[0])
        elif radSel == 1: self.widgetFrame.configure(text=
                        self.i18n.WIDGET_LABEL + self.i18n.colorsIn[1])
        elif radSel == 2: self.widgetFrame.configure(text=
                        self.i18n.WIDGET_LABEL + self.i18n.colorsIn[2])
```

Note how all of the previously hardcoded English strings have been replaced by calls to the instance of our new I18N class. An example is self.win.title(self.i18n.title).

What this gives us is the ability to internationalize our GUI. We simply have to use the same variable names and combine them by passing in a parameter to select the language we wish to display.

We could change languages on the fly as part of the GUI as well, or we could read the local PC settings and decide which language our GUI text should display according to those settings.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_283_0.png)

An example of how to read the local settings is covered in the next recipe, Localizing the GUI.

We can now implement the translation to German by simply filling in the variable names with the corresponding words:

```
class I18N():
    '''Internationalization'''
    def __init__(self, language):
        if  language == 'en': self.resourceLanguageEnglish()
        elif language == 'de': self.resourceLanguageGerman()
```
