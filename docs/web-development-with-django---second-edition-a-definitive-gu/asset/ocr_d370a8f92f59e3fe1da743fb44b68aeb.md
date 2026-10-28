+   6. Migrate your model to the database by running the following two commands. These will first create a Django migrations file and then create a table in your database:

```
```python
python manage.py makemigrations
python manage.py migrate
```
```

+   7. Now, with the database model in place, let’s create a new form that we will use to capture information pertaining to the books, such as the book title, author, and ISBN. For this, create a new file named `forms.py` under the `book_management` directory and add the following code inside it:

```
```python
from django import forms

from .models import Book
class BookForm(forms.ModelForm):
    class Meta:
        model = Book
        fields = ['name', 'author']
```
```

In the preceding code snippet, we first imported Django’s `forms` module, which will allow us to easily create forms and also provide the form’s rendering capability. The next line imports the model that will store the data for the form:

```
```python
from django import forms
from .models import Book
```
```

In the next line, we created a new class named BookForm, which inherits from ModelForm. This is nothing but a class that maps the fields of the model to the form. To successfully achieve this mapping between the model and the form, we defined a new subclass named Meta under the BookForm class and set the attribute model to point to the Book model and the attribute fields to the list of fields that you want to display in the form:

```
```python
class Meta:
    model = Book
    fields = ['name', 'author']
```
```

This allows ModelForm to render the correct form of HTML when expected to do so. The ModelForm class provides a built-in `Form.save()` method, which, when used, writes the data in the form to the database and helps avoid having to write redundant code.

+   8. Now that you have both your model and the form ready, go ahead and implement a view that will render the form and accept input from the user. For this, open `views.py` under the `book_management` directory and add the following lines of code to the file:

```
```python
from django.http import HttpResponse
from django.views.generic.edit import FormView
from django.views import View
```
```