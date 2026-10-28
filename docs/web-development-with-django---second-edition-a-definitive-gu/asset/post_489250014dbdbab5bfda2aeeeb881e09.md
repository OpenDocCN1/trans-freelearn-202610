
Note that the uploaded file and its metadata are being accessed from the `request.FILES` dictionary using the key that matches the name given to the file input (in our case, this is `file_upload`). You can save and close `views.py`. It should now look like this: `https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.03/media_project/media_example/views.py`.

+   8. Start the Django dev server if it’s not already running, then navigate to `http://127.0.0.1:8000/media-example/`. You should see the file upload field and the **Submit** button, as can be seen here:

![](img/4840a43efd335abd10b93708225692d6_405_0.png)

Figure 8.9: The file upload form

Click **Browse…** (or the equivalent on your browser) and select a file to upload. The name of the file will appear in the file input. Then, click **Submit**.

The page will reload, and the form will be empty again. This is normal – in the background, the file should have been saved.

+   9. Try to download the file you uploaded using `MEDIA_URL`. In this example, a file named `cover.jpg` was uploaded. It will be downloadable at `http://127.0.0.1:8000/media/cover.jpg`. Your URL will depend on the name of the file you uploaded:

![](img/4840a43efd335abd10b93708225692d6_406_0.png)

Figure 8.10: Uploaded file visible inside MEDIA_URL

If you upload an image file, HTML file, or another type of file your browser can display, you will be able to view it inside the browser. Otherwise, your browser will just download it to disk again. In both cases, it means the upload was successful.

You can also confirm the upload was successful by looking inside the media directory in the media_project project directory:

![](img/4840a43efd335abd10b93708225692d6_406_1.png)

Figure 8.11: cover.jpg inside the media directory

Figure 8.11 shows the cover.jpg file inside the media directory in PyCharm.

In this exercise, you added an HTML form with enctype set to multipart/form-data so that it allowed file uploads. It contained a file input to select a file to upload. You then added a saving functionality to the media_example view to save the uploaded file to disk.

In the next section, we will look at how to simplify form generation and add validation using Django forms for file upload handling.

## File uploads with Django forms

In Chapter 6, Forms, we saw how Django makes it easy to define forms and automatically render them to HTML. In the previous example, we defined our form manually and wrote the HTML. We can replace this with a Django form and implement the file input with a FileField constructor.

Here’s how FileField is defined on a form:

```
from django import forms

class ExampleForm(forms.Form):
    file_upload = forms.FileField()
```

The FileField constructor can take the following keyword arguments:

- Required: This should be True for required fields and False if the field is optional
- max_length: This refers to the maximum length of the filename of the file being uploaded
- allow_empty_file: A field with this argument is considered to be valid even if the uploaded file is empty (has a size of 0)

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

```
# render a template, the same as for other forms
return render(request, "template.html", {"form": form})
```

When working with uploaded files and forms, you can interact with the uploaded files by accessing them through `request.FILES`, or through `form.cleaned_data`; the values will return to the same object. In the preceding example, we could process the uploaded file like this:

```
if form.is_valid():
    save_file_upload("/path/to/save.jpg",
                     request.FILES["file_upload"])
    return redirect("/success-url/")
```

Or, since they contain the same object, you can use `form.cleaned_data`:

```
if form.is_valid():
    save_file_upload("/path/to/save.jpg",
                     form.cleaned_data["file_upload"])
    return redirect("/success-url/")
```

The data that is saved will be the same.

> **Note**
> 
> In *Chapter 6, Forms*, you experimented with forms and submitted them with invalid values. When the page refreshed to show the form errors, the data that you had previously entered was populated when the page reloaded. This does not occur with file fields; instead, the user will have to navigate and select the file again if the form is invalid.

In the next exercise, we will put what we have seen with `FileFields` into practice by building an example form and then modifying our view to save the file only if the form is valid.

## Exercise 8.04 – file uploads with a Django form

In the previous exercise, you created a form in HTML and used it to upload a file to a Django view. If you tried submitting the form without selecting a file, you would get a Django exception screen. You did not do any validation on the form, so this method is quite fragile.

In this exercise, you will create a Django form with `FileField`, which will allow you to use form validation functions to make the view more robust as well as reduce the amount of code:

+   1. In PyCharm, inside the `media_example` app, create a new file named `forms.py`. It will open automatically. At the start of the file, import the Django forms library:

```
from django import forms
```

Then, create a `forms.Form` subclass, and name it `UploadForm`. Add to it a `FileField` field named `file_upload`. Your class should have this code:

```
class UploadForm(forms.Form):
    file_upload = forms.FileField()
```

You can save and close this file. The complete file should look like this: `https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.04/media_project/media_example/forms.py`.

+   2. Open the `form_example` app’s `views.py` file. At the start of the file, right below the existing import statements, you will need to import your new class like this:

```
from .forms import UploadForm
```

+   3. If you are in the POST branch of the view, `UploadForm` needs to be instantiated with both `request.POST` and `request.FILES`. If you don’t pass in `request.FILES`, then the form instance will not be able to access the uploaded files. Under the `if request.method == "POST"` check, instantiate `UploadForm` with these two arguments:

```
form = UploadForm(request.POST, request.FILES)
```

+   4. The existing lines that define `save_path` and store the file contents can be retained, but they should be indented by one block and put inside a form validity check, so they are only executed if the form is valid. Add the `if form.is_valid():` line, and then indent the other lines, so the code looks like this:

```
if form.is_valid():
    save_path = os.path.join(settings.MEDIA_ROOT,
    request.FILES["file_upload"].name)

    with open(save_path, "wb") as output_file:
        for chunk in
        request.FILES["file_upload"].chunks():
            output_file.write(chunk)
```
