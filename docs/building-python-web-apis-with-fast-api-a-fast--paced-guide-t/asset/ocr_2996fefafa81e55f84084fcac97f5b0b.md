Now that we know how to create a document, let’s look at the methods used for carrying out CRUD operations:

+   - `.insert()` and `.create()`: The `.insert()` and `.create()` methods are called by the document instance to create a new record in the database. You can also choose to use the `.insert_one()` method to add a singular entry to the database.

To insert many entries into the database, the `.insert_many()` method, which takes a list of the document instance, is called, such as the following:

```
event = Event(name="Packt office launch",
location="Hybrid")
await event.create()
await Event.insert_one(event)
```

+   - `.find()` and `.get()`: The `.find()` method is used to find a list of documents matching the search criteria passed as the method argument. The `.get()` method is used to retrieve a single document matching the supplied ID. A single document matching a search criterion can be found using the `.find_one()` method, such as the following:

```
event = await Event.get("74478287284ff")
event = await Event.find(Event.location == "Hybrid").to_list() # Returns a list of matching items
event = await.find_one(Event.location == "Hybrid") # Returns a single event
```

+   - `.save()`, `.update()`, and `.upsert()`: To update a document, any of these methods can be used. The `.update()` method takes an update query, and the `.upsert()` method is used when a document doesn’t match the search criteria. In this chapter, we’ll be making use of the `.update()` method. An update query is an instruction followed by the MongoDB database, such as the following:

```
event = await Event.get("74478287284ff")
update_query = {"$set": {"location": "virtual"}}
await event.update(update_query)
```

In this code block, we first retrieve the event and then create an update query to set the `location` field in the event collection to `virtual`.