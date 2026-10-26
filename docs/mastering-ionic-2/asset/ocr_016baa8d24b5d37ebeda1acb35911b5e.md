related to users should be accessed by that user. To-do items created by one user login should not have permission to other users. So basically it should be filtered according to current users. For this we use custom permissions, for example, while inserting an item we specified the `userId` to which the item is associated:

![](bbox=[0.258, 0.108, 0.741, 0.319])

So now while fetching the items we will pass the current `userId`, which will retrieve data in the current user context:

![](bbox=[0.257, 0.401, 0.744, 0.594])

```
```javascript
todoItems.findAll({user:this.user.id}).watch().subscribe( (items) => {
  // this will only fetch items that
  // login user have created
  console.log(items);
})
```
```

We have tried here to touch each possible section available with IonicDB and get with some sample code. This will really ease the initial learning curve and get you directly started on building the applications using IonicDB. With IonicDB we can also integrate that in our backend NodeJS application.