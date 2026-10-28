This can be used anywhere that you have access to the instance and field, such as in a view or a template. The following example is in a view:

```
instance = ExampleModel.objects.first()
url = instance.file_field.url  # Get the URL
```

Or in a template (assuming instance has been passed to the template context):

```
<img src="{{ instance.file_field.url }}">
```

In the next exercise, we will create a new model with FileField and ImageField and then show how Django can automatically save these. We’ll also demonstrate how to retrieve the URL for an uploaded file.

# Exercise 8.06 – FileField and ImageField on models

In this exercise, we will create a model with FileField and ImageField. After doing this, we will have to generate a migration and apply it. We’ll then change UploadForm we’ve been using, so it has both FileField and ImageField. The media_example view will be updated to store the uploaded files onto the model instance. Finally, we’ll add <img> into the example template to show the previously uploaded image:

+   1. In PyCharm, open the media_example app’s models.py file. Create a new model called ExampleModel, with two fields: ImageField named image_field, and FileField called file_field. ImageField should have upload_to set to images/, and FileField should have upload_to set to files/. The finished model should look like this:

```
class ExampleModel(models.Model):
    image_field = models.ImageField(upload_to="images/")
    file_field = models.FileField(upload_to="files/")
```

Your models.py file should now look like this: https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.06/media_project/media_example/models.py.

+   2. Open a terminal and navigate to the media_project project directory. Make sure your bookr virtual environment is active. Run the makemigrations management command to generate the migrations for this new model (for Windows, you can use python instead of python3 in the following code):

```
python3 manage.py makemigrations
```

> Note
> To learn how to create and activate a virtual environment, refer to the Preface section of the book.