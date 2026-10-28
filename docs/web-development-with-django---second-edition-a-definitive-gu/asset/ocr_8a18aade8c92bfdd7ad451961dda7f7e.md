```
fields = ['name', 'author']
template_name = 'book_form.html'
success_url = '/book_management/entry_success'
```

In the preceding code snippet, we used the built-in UpdateView template, which allows us to update the stored records. Therefore fields attribute here should take in the name of the fields that we would like to allow the user to update.

Once the view is created, the next step is to add the URL mapping. To do this, open the urls.py file under the book_management directory and add the following lines of code:

```
urlpatterns = [
    path('book_record_update/<int:pk>',
        BookUpdateView.as_view(), name='book_update')
]
```

In this example, we appended <int:pk> to the URL field. This signifies the field input we will have to retrieve the record for. Inside Django models, Django inserts a primary key of the integer type, which uniquely identifies the records. Inside the URL mapping, this is the field that we asked to insert.

Now, when we try to open http://localhost:8080/book_management/book_record_update/1, it should show us a record of the first record that we inserted into our database and allow us to edit it:

![](bbox=[0.2, 0.52, 0.8, 0.7])

Figure 11.8 – The view displaying the book record update template based on the Update view

# The Delete view

The Delete view, as the name suggests, is a view that deletes the record from our database. To implement such a view for our Book model, we will need to open the views.py file under the book_management directory and add the following code snippet to it:

```
from django.views.generic.edit import DeleteView
from .models import Book
```