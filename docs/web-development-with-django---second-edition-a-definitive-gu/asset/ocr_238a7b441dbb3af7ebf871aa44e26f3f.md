# File uploads with Django forms

In Chapter 6, Forms, we saw how Django makes it easy to define forms and automatically render them to HTML. In the previous example, we defined our form manually and wrote the HTML. We can replace this with a Django form and implement the file input with a FileField constructor.

Here’s how FileField is defined on a form:

```
from django import forms

class ExampleForm(forms.Form):
    file_upload = forms.FileField()
```

The FileField constructor can take the following keyword arguments:

+   - Required: This should be True for required fields and False if the field is optional
+   - max_length: This refers to the maximum length of the filename of the file being uploaded
+   - allow_empty_file: A field with this argument is considered to be valid even if the uploaded file is empty (has a size of 0)

Apart from these three keyword arguments, the constructor can also accept the standard Field arguments, such as widget. The default widget class for FileField is ClearableFileInput; this is a file input that can display a checkbox that can be checked to send a null value and clear the saved file on a Model field.

Using a form with a FileField constructor in a view is similar to other forms, but when the form has been submitted (i.e., request.METHOD is POST), then request.FILES should be passed into the form constructor as well. This is because Django needs to access request.FILES to find information about uploaded files when validating the form.

The basic flow in a view function is, therefore, similar to this:

```
def view(request):
    if request.method == "POST":
        # instantiate the form with POST data and files
        form = ExampleForm(request.POST, request.FILES)
        if form.is_valid():
            # process the form and save files
            return redirect("success-url")
    else:
        # instantiate an empty form as we've seen before
        form = ExampleForm()
```