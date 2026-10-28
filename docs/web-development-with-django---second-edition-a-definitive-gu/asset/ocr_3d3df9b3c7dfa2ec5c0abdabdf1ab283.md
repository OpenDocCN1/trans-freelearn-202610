In this example, we’re having PIL store to a temporary location with the `Image.save()` method, and then re-opening the file.

This method works but is not ideal, as it involves writing the file to disk and then reading it out again, which can sometimes be slow. Instead, we can perform this whole process in memory.

> **Note**
> 
> `io.BytesIO` and `io.StringIO` are useful objects. They behave like files but exist in memory only. BytesIO is used for storing raw bytes, and StringIO accepts Python 3’s native Unicode strings. You can use read, write, and seek, just like a normal file. Unlike a normal file, though, they don’t get written to disk and, instead, will disappear when your program terminates, or they go out of scope and are garbage collected. They are very useful if a function wants to write to something like a file, but you want to access the data immediately.

First, we will save the image data to an `io.BytesIO` object. Then, we’ll wrap the BytesIO object in a `django.core.files.images.ImageFile` instance (a subclass of File that is specifically for images and provides the width and height attributes). Once we have this ImageFile instance, we can use it in the save method of ImageField.

> **Note**
> 
> `ImageFile` is a file or file-like wrapper just like `File`. It provides two extra attributes: width and height. ImageFile does not generate any errors if you use it to wrap a non-image. For example, you could use `open()` to open a text file and pass the file handle to the ImageFile constructor without issue. You can check whether the image file you passed in was valid by trying to access the width or height attributes: if these are None, then PIL was unable to decode the image data. You can check for the validity of these values yourself and throw an exception if they are None.

Let’s have a look at this in practice in a view:

```
```python
from io import BytesIO
from PIL import Image
from django.core.files.images import ImageFile


def index(request, pk):
    # trim out logic for checking if method is POST

    # get a Model instance, or create a new one
    m = ExampleModel.objects.get(pk=pk)

    # store the uploaded image in a variable for shorter
```
```