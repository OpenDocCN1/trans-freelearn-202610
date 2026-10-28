After clicking **Books**, as shown in the preceding screenshot, you will see the following on your screen:

![](bbox=[0.19, 0.12, 0.78, 0.65])

Figure 4.29 – The Book change page

In this instance, selecting the book *Architects of Intelligence* will take us to the URL http://127.0.0.1:8000/admin/reviews/book/3/change/. In the previous example, all the model fields were rendered as simple HTML text widgets. The rendering of some other subclasses of `django.db.models.Field` used in `models.Book` are worthy of closer examination: