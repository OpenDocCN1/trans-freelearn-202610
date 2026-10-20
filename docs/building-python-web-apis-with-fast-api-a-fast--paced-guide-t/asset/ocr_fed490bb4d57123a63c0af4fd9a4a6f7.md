```
user: str = Depends(authenticate)) -> Event:
    event = await event_database.get(id)
    if event.creator != user:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Operation not allowed"
        )
```

In the preceding code block, the route function checks whether the current user can edit an event before proceeding, otherwise, it raises an HTTP 400 bad request exception.
Here's an example using a different user:

```
$ curl -X 'PUT' \
  'http://0.0.0.0:8080/event/6265a83fc823a3c912830074' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZmFzdGFwaUBwYWNRdC5jb20iLCJleHBpcmVzIjoxNjUwDMzOTc2LjlJ2NzgzMX0.MMRT6pwEDBVHTU5C1a6MV8j9wCfWhqbza9NBpZz08xE' \
  -H 'Content-Type: application/json' \
  -d '{
"title": "FastAPI Book Launch"
}'
```

Here's the response:

```
{
  "detail": "Operation not allowed"
}
```

Lastly, let's update the DELETE route:

```
@event_router.delete("/{id}")
async def delete_event(id: PydanticObjectId, user: str = Depends(authenticate)):
    event = await event_database.get(id)
    if event.creator != user:
        raise HTTPException(
```