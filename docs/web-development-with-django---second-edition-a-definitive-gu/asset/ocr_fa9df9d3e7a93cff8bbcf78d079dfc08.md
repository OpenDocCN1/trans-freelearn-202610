If we tried to render the form now, we wouldn’t be able to submit it, as there’s no submit button (remember that we added submit buttons to our forms manually; they are not part of the Django form). django-crispy-forms also includes layout helpers that can be added to the form. They will be rendered after the other fields. We can add a submit button like this – first, import the Submit class:

```
from crispy_forms.layout import Submit
```

> **Note**
> django-crispy-forms does not properly support using a <button> input to submit a form, but for our purposes, <input type="submit"> is functionally identical.

We then instantiate it and add it to the helper’s inputs in a single line:

```
self.helper.add_input(Submit("submit", "Submit"))
```

The first argument to the Submit constructor is its name, and the second is its label.

django-crispy-forms is aware that we are using Bootstrap and will automatically render the button with the btn btn-primary classes.

The advantage of using a crispy template tag and FormHelper is that it means there is only one place where attributes and the behavior of the form are defined. We are already defining all the form fields in a Form class; this allows us to define the other attributes of the form in the same place. We can change a form from a GET submission to a POST submission easily here. The FormHelper instance will then automatically know that it needs to add a CSRF token to its HTML output when rendered.

We’ll put all this into practice in the next exercise, where you will install django-crispy-forms, update SearchForm to utilize a form helper, and then render it using the crispy template tag.

# Exercise 15.04 – using Django Crispy Forms with SearchForm

In this exercise, you will install django-crispy-forms and then convert SearchForm to be usable with the crispy template tag. This will be done by adding an __init__ method and building a FormHelper instance inside it:

+   1. In a terminal, make sure you have activated the bookr virtual environment, and then run this command to install django-crispy-forms using pip3:

```
pip3 install django-crispy-forms
```

> **Note**
> For Windows, you can use pip instead of pip3 in the preceding command.