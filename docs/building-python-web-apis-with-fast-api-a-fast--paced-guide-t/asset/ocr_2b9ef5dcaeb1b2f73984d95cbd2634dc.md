This field will enable us to restrict the operations performed on an event to the user alone.

Next, let’s modify the POST route to update the creator field when creating a new event in routes/events.py:

```
@event_router.post("/new")
async def create_event(body: Event, user: str = Depends(authenticate)) -> dict:
    body.creator = user
    await event_database.save(body)
    return {
        "message": "Event created successfully"
    }
```

In the preceding code block, we have updated the POST route to add the current user’s email as the creator of the event. If you create a new event, the event is stored with the creator's email:

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXV CJ9.eyJlc2VyIjoicmVhZGVyQHBhY2t0LmNvbSIImV4cGlyZXMiOjE2NTA4MzI5NjqQuMTU3MjQ4fQ.RxR1TYMx91JtVMNzYcT7718xXWX7skTCfWbnJxyf6fU' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI Book Launch",
"image": "https://linktomyimage.com/image.png",
"description": "We will be discussing the contents of the FastAPI book in this event.Ensure to come with your own copy to win gifts!",
"tags": [
    "python",
    "fastapi",
    "book",
    "launch"
],
"location": "Google Meet"
```