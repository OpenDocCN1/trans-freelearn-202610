Now, let’s create a new event from the command line:

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXV CJ9.eyJlc2VyIjoicmVhZGVyQHBhY2t0LmNvbSIImV4cGlyZXMiOjE2NTA4Mjk xODMuNTgTg3NjAyfQ.MOXjI5GXnyzGNftdlxDGyM119_L1luPq8yCxBHepf04' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI Book Launch CLI",
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

In the request sent here, the Authorization: Bearer header is sent as well to inform the application that we are authorized to perform this action. The response gotten is the following:

```
{ "message": "Event created successfully" }
```

If we try to create an event without passing the authorization header with a valid token, an HTTP 401 Unauthorized error is returned:

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/event/new' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"title": "FastAPI BookLaunch",
```