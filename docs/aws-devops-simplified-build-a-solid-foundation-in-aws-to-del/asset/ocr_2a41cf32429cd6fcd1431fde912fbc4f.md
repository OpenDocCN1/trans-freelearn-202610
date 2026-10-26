# Initializing the Flask application and MongoDB connection

> **Note**
> 
> All application code discussed here onward is available inside your Cloud9 IDE at the `chapter-7/chapter-7-flask-app` path or can be referenced in this book’s GitHub repository: https://github.com/PacktPublishing/AWS-DevOps-Simplified/tree/main/chapter-7/chapter-7-flask-app.

We can use existing Python modules to interact with Flask and MongoDB. These dependencies have been defined in the `requirements.txt` file inside the application directory:

```
from flask import Flask, render_template, request, url_for, redirect
from pymongo import MongoClient
from markupsafe import escape
from bson.objectid import ObjectId

# Initialize global variables for MongoDB connection

app = Flask(__name__)
mongodb_client = MongoClient('localhost', 27017)
todos_db = mongodb_client.flask_db
todos_collection = todos_db["todos"]
...
```

After importing the necessary modules, we initialize a DB client, database, and corresponding collection that will host our JSON documents in MongoDB.

Now, we can move on to defining the view functions and the related HTTP methods using Flask decorators:

```
... 
@app.route("/", methods = ('GET', 'POST'))
def todos():
    if (request.method == "POST"):
        todo_item = request.form["todo"]
        todo_type = request.form["tasktype"]
        todos_collection.insert_one({"todo": todo_item,"tasktype": todo_type})
        return redirect(url_for('todos'))
    all_todos = todos_collection.find()
    return render_template('index.html', all_todos = all_todos)

@app.post('/<id>/delete')
def delete(id):
```