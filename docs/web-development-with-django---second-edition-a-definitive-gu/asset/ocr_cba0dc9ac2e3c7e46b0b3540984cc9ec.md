Back to `File` and `ContentFile`: the one to use depends on what you want to store in `FileField`.

`File` is used as a wrapper around a Python file object, and you should use it if you have an existing file or file-like object that you want to save. File-like objects include `io.BytesIO` or `io.StringIO` instances. To instantiate a `File` instance, just pass the native file object to the constructor, for example:

```
f = open("/path/to/file.txt", "rb")
file_wrapper = File(f)
```

Use `ContentFile` when you already have some data loaded, either a `str` object or a `bytes` object. Pass the data to the `ContentFile` constructor:

```
string_content = ContentFile("A string value")
bytes_content = ContentFile(b"A bytes value")
```

Now that you have either a `File` instance or a `ContentFile` instance, saving the data to `FileField` is easy, using the `save` method:

```
m = ExampleModel.objects.first()
with open("/path/to/file.txt") as f:
    file_wrapper = File(f)
    m.file_field.save("file.txt", f)
```

Since we did not pass a value for `save` to the save method, it will default to `True`, so the model instance is automatically persisted to the database.

Next, we will look at how to store an image that has been manipulated with a PIL back to an image field.

# Writing PIL images to ImageField

In *Exercise 8.05 – image uploads using Django forms*, you used PIL to resize an image and save it to disk. When working with a model, you might want to perform a similar operation but have Django handle the file storage using `ImageField` so that you don’t have to do it manually. As in the exercise, you could save the image to disk and then use the `File` class to wrap the stored path – something like this:

```
image = Image.open(request.FILES["image_field"])
image.thumbnail((150, 150))
image.save("/tmp/thumbnail.jpg")  # save thumbnail to temp
                             # location

with open("/tmp/thumbnail.jpg", "rb") as f:
    image_wrapper = File(f)
    m.image_field.save("thumbnail.jpg", image_wrapper)

os.unlink("/tmp/thumbnail.jpg")  # clean up temp file
```