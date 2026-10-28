```
...
]
```

Since we are using a class-based view, we have to use the class name along with the as_view() method.

+   3. Once all the preceding modifications are completed, wait till the Django service restarts or start the server with the python manage.py runserver command, and then open the API at http://127.0.0.1:8000/api/all_books/ in the web browser. You should see something like Figure 12.3:

![](bbox=[0.115, 0.285, 0.852, 0.638])

Figure 12.3: List of books shown in the all_books endpoint

Similar to what we saw in Exercise 12.02, Creating an API view to display a list of books, this is a list of all books present in the book review application. In this exercise, we used model serializers to simplify our code, and the generic class-based ListAPIView to return a list of the books in our database. In the next section, you will implement an API endpoint to return a list of top contributors.