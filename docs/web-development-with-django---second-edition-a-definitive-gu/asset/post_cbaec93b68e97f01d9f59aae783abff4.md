
So far, we have learned about multi-field validations, and how to implement them to make sure that we can validate the form in one go rather than highlighting errors one by one.

In the next section, we will cover how to add placeholders and initial values to the form.

## Adding placeholders and initial values

There are two things our first manually built form had that our current Django form still does not have – placeholders and initial values. Adding placeholders is simple; they are just added as attributes to the widget constructor for the form field. This is similar to what we've already seen for setting the type of DateField in our previous examples.

Here's an example:

```
class ExampleForm(forms.Form):
    text_field = forms.CharField(
        widget=forms.TextInput(attrs={"placeholder": "Text Placeholder"})
    )
    password_field = forms.CharField(
        widget=forms.PasswordInput(attrs={"placeholder": "Password Placeholder"})
    )
    email_field = forms.EmailField(
        widget=forms.EmailInput(attrs={"placeholder": "Email Placeholder"})
    )
    text_area = forms.CharField(
        widget=forms.Textarea(attrs={"placeholder": "Text Area Placeholder"})
    )
```

Values in `request.POST` will override values in `initial`. This means that even if we have an initial value for a required field, if it's left blank when submitted, then it will not validate. The field will not fall back to the value in `initial`.

Whether you decide to set initial values in the Form class itself or the view is up to you and depends on your use case. If you had a form that was used in multiple views but usually had the same value, it would be better to set the `initial` value in the form. Otherwise, it can be more flexible to use `setting` in the view.

In the next exercise, we will add placeholders and initial values to the `OrderForm` class from the previous exercise.

## Exercise 7.02 – placeholders and initial values

In this exercise, you will enhance the `OrderForm` class by adding placeholder text. You will simulate passing an initial email address to the form. It will be a hardcoded address but once the user can log in, it could be an email address associated with their account – you will learn about sessions and authentication in *Chapter 9, Sessions and Authentication*:

+   1. In PyCharm, open the `reviews` app's `forms.py` file. You will add placeholders to the `magazine_count`, `book_count`, and `email` fields on the `OrderForm`, which means also setting widget.
+   
+   To the `magazine_count` field, add a `NumberInput` widget with `placeholder` in the `attrs` dictionary. This `placeholder` should be set to **Number of Magazines**. Write the following code:

```
magazine_count = forms.IntegerField(
    min_value=0,
    max_value=80,
    widget=forms.NumberInput(attrs={"placeholder":
    "Number of Magazines"}),
)
```

+   2. Add a placeholder to the `book_count` field in the same manner. The placeholder text should be **Number of Books**:

```
book_count = forms.IntegerField(
    min_value=0,
    max_value=50,
    widget=forms.NumberInput(attrs={"placeholder":
    "Number of Books"}),
)
```

3. The final change to OrderForm is to add a placeholder to the email field. This time, the widget is EmailInput. The placeholder text should be Your company email address:

```
email = forms.EmailField(
    required=False,
    validators=[validate_email_domain],
    widget=forms.EmailInput(attrs={"placeholder":
    "Your company email address"}),
)
```

Note that the clean_email and clean methods should remain as they were in Exercise 7.01 – custom clean and validation methods. Save the file.

4. Open the form_project app’s views.py file. In the form_example view function, create a new dictionary variable called initial with one key, email, like this:

```
initial = {"email": "user@example.com"}
```

5. In the two places that you are instantiating OrderForm, also pass in the initial variable using the initial kwarg. The first instance is as follows:

```
form = OrderForm(request.POST, initial=initial)
```

The second instance is as follows:

```
form = OrderForm(initial=initial)
```

The complete code for views.py can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter07/Exercise7.02/form_project/form_example/views.py. Save the views.py file.

+   6. Start the Django dev server if it is not already running. Browse to http://127.0.0.1:8000/form-example/ in your browser. You should see that your form now has placeholders and an initial value set:

![](img/4840a43efd335abd10b93708225692d6_362_0.png)

Figure 7.17 – Order form with initial values and placeholders

In this exercise, we added placeholders to form fields. This was done by setting a form widget when defining the form field on the form class and setting a placeholder value in the attrs dictionary. We also set an initial value for the form using a dictionary and passed it to the form instance using the initial kwarg.

In the next section, we will talk about how to work with Django models using data from forms, and how ModelForm makes this easier.

## Creating or editing Django models

We’ve seen how to define a form, and in Chapter 2, Models and Migrations, you learned how to create Django model instances. By using these things together, you can build a view that displays a form and also saves a model instance to the database. This gives you an easy way to save data without having to write a lot of boilerplate code or create custom forms. In Bookr, we will use this method to allow users to add reviews without requiring access to the Django admin site. Without using ModelForm, you can do something like this:

+   You can create a form based on an existing model; for example, Publisher. The form would be called PublisherForm.

+   - You can manually define the fields on PublisherForm, using the same rules defined on the Publisher model, as shown here:

```
class PublisherForm(forms.Form):
    name = forms.CharField(max_length=50)
    website = forms.URLField()
    ...
```

+   - In the view, the initial values would be retrieved from the model queried from the database, then passed to the form using the initial argument. If you were creating a new instance, the initial value would be blank – something like this:

```
if create:
    initial = {}
else:
    publisher = Publisher.objects.get(pk=pk)
    initial = {"name": publisher.name, "website":
    publisher.website, ...}

form = PublisherForm(initial=initial)
```

+   - Then, in the POST flow of the view, you can either create or update the model based on cleaned_data:

```
form = PublisherForm(request.POST, initial=initial)
if create:
    publisher = Publisher()
else:
    publisher = Publisher.objects.get(pk=pk)

publisher.name = form.cleaned_data['name']
publisher.website = forms.cleaned_data['website']
...
publisher.save()
```

This is a lot of work, and we have to consider how much duplicated logic we have. For example, we’re defining the length of the name in the name form field. If we made a mistake here, we could allow a longer name in the field than the model allows. We also have to remember to set all the fields in the `initial` dictionary, as well as set the values on the new or updated model with `cleaned_data` from the form. There are many opportunities to make mistakes here, as well as remembering to add or remove field setting data for each of these steps if the model changes. All this code would have to be duplicated for each Django model you work with as well, expounding the duplication problem.

## The ModelForm class

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
