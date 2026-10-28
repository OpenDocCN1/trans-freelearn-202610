+   4. Start the Django service with the `python manage.py runserver` command and go to `http://127.0.0.1:8000/api/first_api_view/` to make your first API request. Your screen should appear as in Figure 12.1:

![](bbox=[0.117, 0.17, 0.855, 0.398])

Figure 12.1 – First Api View with the number of books

Calling this URL endpoint made a default GET request to the API endpoint, which returned a JSON key-value pair (`"num_books": 0`). Also, notice how DRF provides a nice interface to view and interact with the APIs.

+   5. We could also use the Linux `curl` (client URL) command to send an HTTP request as follows:

```
```bash
curl http://127.0.0.1:8000/api/first_api_view/
{"num_books":18}
```
```

Alternatively, if you are using Windows 10, you can make an equivalent HTTP request with `curl.exe` from Command Prompt as follows:

```
```bash
curl.exe http://127.0.0.1:8000/api/first_api_view/
```
```

In this exercise, we learned how to create an API view using DRF and a simple functional view. We will now look at a more elegant way to convert between information stored in the database and what gets returned by our API using serializers.