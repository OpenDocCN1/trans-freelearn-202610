In this code block, we start by importing the dependencies required for initializing the database. Then, we define the Settings class, which has a DATABASE_URL value that is read from the env_file environment defined in the Config subclass. We also define an initialize_database method to initialize the database.

The init_beanie method takes the database client, which is the mongo version of the engine created in the SQLModel section, and a list of documents.

3. Let’s update the model files in the models directory to include the MongoDB documents. In models/events.py, replace the contents with the following:

```
from beanie import Document
from typing import Optional, List

class Event(Document):
    title: str
    image: str
    description: str
    tags: List[str]
    location: str

class Config:
    schema_extra = {
        "example": {
            "title": "FastAPI Book Launch",
            "image": "https://linktomyimage.com/image.png",
            "description": "We will be discussing the contents of the FastAPI book in this event. Ensure to come with your own copy to win gifts!",
            "tags": ["python", "fastapi", "book", "launch"],
            "location": "Google Meet"
        }
    }
```