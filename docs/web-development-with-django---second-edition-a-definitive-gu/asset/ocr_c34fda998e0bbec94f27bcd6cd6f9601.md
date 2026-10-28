```
def download_view(request, relative_path):
    if request.user.is_anonymous:
        raise PermissionDenied
    full_path = os.path.join(settings.MEDIA_ROOT,
        "protected", relative_path)
    file_handle = open(full_path, "rb")
    return FileResponse(file_handle)  # Django sends the
                                    file then closes
                                    the handle
```

The URL mapping to this view could be like this, using the path path converter inside your urls.py file:

```
urlpatterns = [
    ...
    path("downloads/<path:relative_path>",
    views.download_view)
]
```

There are many ways that you could choose to implement a view that sends files. The important thing is that you use the FileResponse class, which is designed to stream the file to the client in chunks instead of loading it all into memory. This will reduce the load on the server and lessen the impact on resource usage if you must resort to sending files with Django.

Now that we know how to upload and process images, let’s look at how to store them on models.

# Storing files on model instances

So far, we have manually managed the uploading and saving of files. You can also associate a file with a model instance by assigning the path to which it was saved to CharField. However, as with much of Django, this capability (and more) is already provided with the models.FileField class. A FileField instance does not actually store the file data; instead, they store the path where the file is stored (like CharField would), but it also provides helper methods. These methods assist with loading files (so you don’t have to open them manually) and generating disk paths for you based on the ID of the instance (or other attributes).