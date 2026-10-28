+   3. Open the media_example app’s views.py file. Since you no longer need to reference ExampleModel directly, you can remove its import at the top of the file. Remove the following line:

```
from .models import ExampleModel
```

+   4. In the media_example view, remove the entirety of the form.is_valid() branch and replace it with a single line:

```
instance = form.save()
```

The form’s save method will handle persisting the instance to the database and saving the files. It will return an instance of ExampleModel, the same as the other ModelForm we have worked with in Chapter 7, Advanced Form Validation and Model Forms.

After completing this step, your media_example function should look like https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.07/media_project/media_example/views.py. Save and close views.py.

+   5. Start the Django dev server if it’s not already running, then navigate to http://127.0.0.1:8000/media-example/. You should see the form rendered with two fields, Image field and File field (Figure 8.24):

![](bbox=[0.12, 0.49, 0.85, 0.64])

Figure 8.24: UploadForm as a ModelForm rendered in the browser

Note that the names of these fields now match those of the model rather than the form, since the form just uses the model’s fields.