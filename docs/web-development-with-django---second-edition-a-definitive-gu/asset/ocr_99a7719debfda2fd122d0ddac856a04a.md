This is a lot of work, and we have to consider how much duplicated logic we have. For example, we’re defining the length of the name in the name form field. If we made a mistake here, we could allow a longer name in the field than the model allows. We also have to remember to set all the fields in the `initial` dictionary, as well as set the values on the new or updated model with `cleaned_data` from the form. There are many opportunities to make mistakes here, as well as remembering to add or remove field setting data for each of these steps if the model changes. All this code would have to be duplicated for each Django model you work with as well, expounding the duplication problem.

# The ModelForm class

Luckily, Django provides a method of building Model instances from forms much more simply, with the ModelForm class. A ModelForm is a form that is built automatically from a particular model. It will inherit the validation rules from the model (such as whether fields are required or the maximum length of CharField instances, and so on). It provides an extra `__init__` argument (called instance) for automatically populating the initial values from an existing model. It also adds a save method to automatically persist the form data to the database. All you need to do to set up ModelForm is specify its model and what fields should be used: this is done on the class Meta attribute of the form class. Let’s see how to build a form from Publisher.

Inside the file that contains the form (for example, the `forms.py` file we have been working with), the only change is that the model must be imported:

```
```python
from .models import Publisher
```
```

Then, the Form class can be defined. The class requires a `class Meta` attribute, which, in turn, must define a model attribute and either the fields or excludes attributes:

```
```python
class PublisherForm(forms.ModelForm):
    class Meta:
        model = Publisher
        fields = ("name", "website", "email")
```
```

`fields` is a list or tuple of the fields to include in the form. When manually setting the list of fields, if you add extra fields to the model, you must also add their name here to have them displayed on the form.

You can also use the special `__all__` value instead of a list or tuple to automatically include all the fields, like this:

```
```python
class PublisherForm(forms.ModelForm):
    class Meta:
        model = Publisher
        fields = "__all__"
```
```

If the model field has its editable attribute set to False, then it will not be automatically included.