```
return False

async def get_all(self) -> List[Any]:
    docs = await self.model.find_all().to_list()
    return docs
```

The first method, get(), takes an ID as the method argument and returns a corresponding record from the database, while the get_all() method takes no argument and returns a list of all the records present in the database.

# Update

Let’s create the method to handle the process of updating an existing record:

```
async def update(self, id: PydanticObjectId, body: BaseModel) -> Any:
    doc_id = id
    des_body = body.dict()
    des_body = {k:v for k,v in des_body.items() if v is not None}
    update_query = {"$set": {
        field: value for field, value in des_body.items()
    }}

    doc = await self.get(doc_id)
    if not doc:
        return False
    await doc.update(update_query)
    return doc
```

In this code block, the update method takes an ID and the Pydantic schema responsible, which will contain the fields updated from the PUT request sent by the client. The updated request body is first parsed into a dictionary and then filtered to remove None values. Once this has been done, it is then inserted into an update query, which is finally executed by Beanie’s update() method.