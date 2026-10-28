# URL Mapping, Views, and Templates

In the previous chapter, we were introduced to databases, and we learned how to store, retrieve, update, and delete records from a database. We also learned how to create Django models and apply database migrations.

However, these database operations alone cannot display an application’s data to a user. We need a way to display all the stored information in a meaningful way to the user – for example, displaying all the books present in our Bookr application’s database, in a browser, and in a presentable format. This is where Django views, templates, and URL mapping come into play. Views are the part of a Django application that takes in a web request and provides a web response. For example, a web request could be a user trying to view a website by entering the website address, and a web response could be the website’s home page loading in the user’s browser. Views are one of the most important parts of a Django application, where the application logic is written. This application logic controls interactions with the database, such as creating, reading, updating, or deleting records from the database. It also controls how the data is displayed to the user. This is done with the help of Django HTML templates.

This chapter introduces you to three core concepts of Django: views, templates, and URL mapping. You will start by exploring the two main types of views in Django: function-based views and class-based views. Next, you will learn the basics of Django template language and template inheritance. Using these concepts, you will create a page to display the list of all the books in the Bookr application. You will also create another page to display the details, review comments, and ratings of books.

In this chapter, we will study the following topics:

+   - Understanding function-based views
+   - Understanding class-based views
+   - URL configuration
+   - Working with Django templates
+   - Django’s template language