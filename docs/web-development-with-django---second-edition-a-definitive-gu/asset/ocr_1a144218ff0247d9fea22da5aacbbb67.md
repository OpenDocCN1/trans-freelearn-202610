We would prefer not to specify that the title must be serializers.CharField(). It would be easier if the serializer just looked at how title was defined in the model and could figure out what serializer field to use.

This is where model serializers come in. They provide shortcuts to create serializers by utilizing the definition of the fields on the model. Instead of specifying that title should be serialized using CharField, we just tell the model serializer we want to include title, and it uses the CharField serializer because the title field on the model is also CharField.

For example, suppose we wanted to create a serializer for the Contributor model in models.py. Instead of specifying the types of serializers that should be used for each field, we can give it a list of the field names and let it figure out the rest:

```
from rest_framework import serializers

from .models import Contributor

class ContributorSerializer(serializers.ModelSerializer):
    class Meta:
        model = Contributor
        fields = ['first_names', 'last_names', 'email']
```

In the following exercise, we will see how we can use a model serializer to avoid the duplication of code in the preceding classes.

# Exercise 12.03 – creating class-based API views and model serializers

In this exercise, you will create class-based views to display a list of all books while using model serializers:

+   1. Open the bookr/reviews/serializers.py file, remove any pre-existing code, and replace it with the following code:

```
from rest_framework import serializers
from .models import Book, Publisher

class PublisherSerializer(
    serializers.ModelSerializer):
    class Meta:
```