# Activity 13.01 – exporting the books read by a user as an XLSLX file

In this activity, you will implement a new API endpoint inside Bookr that will allow your users to export and download a list of books they have read as an XLSX file:

+   1. Install the `XlsxWriter` library.
+   2. Inside the `utils.py` file you created under the `bookr` application, create a new function that will help in fetching the list of books that have been read by the user.
+   3. Inside the `views.py` file under the `bookr` directory, create a new view function that will allow the user to download their reading history in XLSX format.
+   4. To create an XLSX file inside the view function, first, create a BytesIO-based in-memory file that can be used to store the data from the `XlsxWriter` library.
+   5. Read the data stored inside the in-memory file using the `getvalue()` method of the temporary file object.
+   6. Finally, create a new `HttpResponse` instance with the `'application/vnd.ms-excel'` content type header, and then write the data obtained in step 5 to the response object.
+   7. With the response object prepared, return the response object from the view function.
+   8. With the view function ready, map it to a URL endpoint that can be visited by a user to download their book-reading history.

Once you have mapped the URL endpoint, start the application and log in to it with your user account. Once you’ve done this, visit the URL endpoint you just created, and if upon visiting the URL endpoint your browser starts to download an Excel file, you have successfully completed the activity.

> **Important note**
> The solution for this activity can be found on https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.