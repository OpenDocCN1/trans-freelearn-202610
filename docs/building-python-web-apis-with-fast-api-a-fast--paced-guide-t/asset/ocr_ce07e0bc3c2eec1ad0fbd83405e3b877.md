```
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
}'
```

Here's the response:

```
$ {
  "detail": "Not authenticated"
}
```

Now that we have successfully protected the routes, let's update the protected routes as follows:

+   - POST route: Add the event created to the list of events owned by the user.
+   - UPDATE route: Modify the route to ensure only the event created by the user can be updated.
+   - DELETE route: Modify the route to ensure only the event created by the user can be deleted.

In the previous section, we successfully injected the authentication dependencies to our route operations. To easily identify events and prevent a user from deleting another user's event, we'll update the event document class as well as the routes.

# Updating the event document class and routes

Add the creator field to the Event document class in models/events.py:

```
class Event(Document):
    creator: Optional[str]
```