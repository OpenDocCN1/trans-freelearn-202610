```
}
]
}
}
```

In the preceding code block, we have defined a new model, TodoItems, which returns a list of variables contained in the TodoItem model. Let’s update our route in todo.py by adding a response model to it:

```
from model import Todo, TodoItem, TodoItems
...
@todo_router.get("/todo", response_model=TodoItems)
async def retrieve_todo() -> dict:
    return {
        "todos": todo_list
    }
```

Activate your virtual environment and start your application:

```
$ source venv/bin/activate
(venv)$ uvicorn api:app --host=0.0.0.0 --port 8000 --reload
```

Next, add a new to-do:

```
(venv)$ curl -X 'POST' \
'http://127.0.0.1:8000/todo' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"id": 1,
"item": "This todo will be retrieved without exposing my ID!"
}'
```