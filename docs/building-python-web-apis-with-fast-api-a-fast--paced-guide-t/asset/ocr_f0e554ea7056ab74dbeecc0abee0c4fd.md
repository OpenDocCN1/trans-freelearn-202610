# ReDoc

The ReDoc documentation gives a more detailed and direct presentation of the models, routes, and API. You can access it by appending /redoc to the application address. In your web browser, visit the http://127.0.0.1:8000/redoc URL:

![](bbox=[0.13, 0.19, 0.87, 0.56])

Figure 2.3 – The ReDoc-powered documentation portal

To correctly generate JSON Schema, you can set examples of how a user will fill data in the model. An example is set by embedding a Config class into a model class. Let’s add an example schema in our Todo model:

```
class Todo(BaseModel):
    id: int
    item: str

    class Config:
        Schema_extra = {
            "Example": {
                "id": 1,
```