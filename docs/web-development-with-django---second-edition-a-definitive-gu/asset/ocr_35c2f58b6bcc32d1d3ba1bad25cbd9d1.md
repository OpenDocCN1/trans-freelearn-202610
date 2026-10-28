# Working with uploaded files in a view

In addition to text data, if a form also contains file uploads, Django will populate the request.FILES attribute with uploaded files. The request.FILES attribute is a dictionary-like object that is keyed on the name attribute given to the file input.

In the form example in the previous section, the file input had the name file-upload-name. So the file would be accessible in the view using request.FILES["file-upload-name"].

The objects that request.FILES contains are file-like objects (specifically a django.core.files.uploadedfile.UploadedFile instance), so to use them, you must read their data. For example, to get the content of an uploaded file in your view, you can write the following:

```
content = request.FILES["file-upload-name"].read()
```

A more common action is to write the file contents to disk. When files are uploaded, they are stored in a temporary location (in memory if they are under 2.5 MB, otherwise in a temporary file on disk). To store the file data in a known location, the contents must be read and then written to disk at that location. UploadedFile has a chunks method that will read the file data one chunk at a time to prevent too much memory from being used by reading the entirety of the file at once.

So, instead of simply using the read and write functions, use the chunks method to only read small chunks of the file into memory at a time:

```
with open("/path/to/output.jpg", "wb+") as output_file:
    uploaded_file = request.FILES["file-upload-name"]
    for chunk in uploaded_file.chunks():
        output_file.write(chunk)
```

Note that in some of the upcoming examples, we will refer to this code as the save_file_upload function. Assume the function is defined like this:

```
def save_file_upload(upload, save_path):
    with open(save_path, "wb+") as output_file:
        for chunk in upload.chunks():
            output_file.write(chunk)
```

The previous example code could then be refactored to call the function:

```
uploaded_file = request.FILES["file-upload-name"]
save_file_upload(uploaded_file, "/path/to/output.jpg")
```