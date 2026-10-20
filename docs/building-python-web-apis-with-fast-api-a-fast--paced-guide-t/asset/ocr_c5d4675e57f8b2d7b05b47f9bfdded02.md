A complete list of HTTP status codes can be found at https://httpstatuses.com/.

The first digit of a status code defines its category. Common status codes include 200 for a successful request, 404 for request not found, and 500 indicating an internal server error.

The standard practice followed in building web applications, irrespective of the framework, is to return appropriate status codes for individual events. A 400 status code shouldn’t be returned for a server error. Likewise, a 200 status code shouldn’t be returned for a failed request operation.

Now that you have learned what status codes are, let’s learn how to build response models in the next section.

# Building response models

We established the purpose of response models at the beginning of this chapter. You also learned how to build models in the previous chapter using Pydantic. Response models are also built on Pydantic but serve a different purpose.

In the definition of route paths, we have the following, for example:

```
@app.get("/todo")
async def retrieve_todo() -> dict:
    return {
        "todos": todo_list
    }
```

The route returns a list of to-dos present in the database. Here’s some example output:

```
{
  "todos": [
    {
      "id": 1,
      "item": "Example schema 1!"
    },
    {
      "id": 2,
      "item": "Example schema 2!"
    },
    {
      "id": 3,
```