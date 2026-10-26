```
}, (error) => {
    console.error(error);
});
```

All the read and write operations return the RxJS observable. subscribe is a method on observable, which takes three functions as parameters:

+   - next(result): callback method that receives collection data
+   - error(error): callback method that receives error data
+   - complete(): callback executed when the result set is iterated completely

If you want to stop receiving emissions you can call the unsubscribe() method. We will be mostly using read methods from our mobile application to retrieve data from the database. Although, we also have multiple write operations that can also be used for user centric applications such as to-do applications.

Let's see some write operations also and how we can use it. Although, in our vPlanet application we haven't used write operations, but let's demonstrate how we can use some write operations for a to-do application:

```
const todoItems = this.db.collection("items");

// Insert a todo item document

let dateTime = new Date().toLocaleString();

todoItems.insert({
  id: "1",
  user: this.user.id,
  text: "Prepare project presentation",
  status: "pending"
  timestamp: dateTime
});

// Update todo item

let dateTime = new Date().toLocaleString();

todoItems.update({
  id: "1",
  user: this.user.id,
  text: "Prepare project presentation",
  status: "completed"
  timestamp: dateTime
});
```

We now have a good idea of how read and write operations work with IonicDB. IonicDB is still in active development, so you can expect some issues, but until now I have had a smooth journey developing vPlanet applications.