You can see that updating FileField on an existing model instance is the same process as setting it on a new instance and if you choose to use a Django form or just access request.FILES directly, the process is just as simple.

Now we’ll look at how to store images on models using ImageField.

# Storing images on model instances

While FileField can store any type of file, including images, there also exists ImageField. As you would expect, this is only for storing images. The relationship between the models’ forms.FileField and forms.ImageField is similar to that between models.FileField and models.ImageField, meaning ImageField extends FileField and adds extra methods for working with images.

The ImageField constructor takes the same arguments as FileField and adds two extra optional arguments:

+   - height_field: This is the name of the field on the model that will be updated with the height of the image every time the model instance is saved

+   - width_field: This is the width counterpart to height_field: the field that stores the width of the image that is updated every time the model instance is saved

Both of these arguments are optional, but the fields they name must exist if used. That is, it is valid to have height_field or width_field unset, but if they are set to the name of a field that does not exist, then an error will occur. The purpose of this is to assist with searching the database for files of a particular dimension.

Here is an example model using ImageField, which updates the image dimension fields:

```
class ExampleModel(models.Model):
    image = models.ImageField(upload_to="images/%Y/%m/%d/",
                              height_field="image_height",
                              width_field="image_width"
                              )
    image_height = models.IntegerField()
    image_width = models.IntegerField()
```