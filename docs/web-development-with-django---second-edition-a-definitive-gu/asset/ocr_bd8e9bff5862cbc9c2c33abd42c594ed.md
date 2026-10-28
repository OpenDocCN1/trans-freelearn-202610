On a successful POST, register a success message that Book was updated, then redirect to the book_detail view.

+   8. Render instance-form.html, passing a context dictionary containing form, model_type, and instance, as you did in Chapter 6, Forms. Also, pass another item, is_file_upload, set to True. This variable will be used in the next step.

+   9. In the instance-form.html template, use the is_file_upload variable to add the correct enctype attribute to the form. This will allow you to switch the modes for the form to enable file uploads when required.

+   10. Finally, add a URL map that maps /books/<pk>/media/ to the book_media view.

When you're finished, you should be able to start the Django dev server and load the book_media view at http://127.0.0.1:8000/books/<pk>/media/, for example, http://127.0.0.1:8000/books/2/media/. You should see BookMediaForm rendered in the browser, like in Figure 8.28:

![](bbox=[0.13, 0.37, 0.83, 0.63])

Figure 8.28: BookMediaForm in the browser

Select a cover image and a sample file for the book. You can use the image at https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Activity8.01/bookr/fixtures/machine-learning-for-algorithmic-trading.png and the PDF at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter08/Activity8.01/bookr/fixtures/machine-learning-for-trading.pdf (or you can use any other image/PDF of your choosing).