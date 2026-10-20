Switch back to the main branch:

```
(venv)$ git checkout main
```

Now that you are back to the original version of the application, let’s incorporate MongoDB as the database platform and implement CRUD operations in the next section.

# Setting up MongoDB

There are a number of libraries that allow us to integrate MongoDB into our FastAPI application. However, we’ll be using Beanie, an asynchronous Object Document Mapper (ODM) library, to execute database operations from our application.

Let’s install the beanie library by running the following command:

```
(venv)$ pip install beanie
```

Before diving into the integration, let’s look at some of the methods from the Beanie library and also how database tables are created in this section.

# Document

In SQL, the data stored in rows and columns are contained in the table. In a NoSQL database, it is called a document. The document represents how the data will be stored in the database collection. Documents are defined the same way a Pydantic model is defined, except that the Document class from the Beanie library is inherited instead.

An example document is defined as follows:

```
from beanie import Document

class Event(Document):
    name: str
    location: str

    class Settings:
        name = "events"
```

The Settings subclass is defined to tell the library to create the collection name passed in the MongoDB database.