In our test, we create a message to be sent to a user, we then use the blockUser() service to block messages from that user, and subscribe to the returned observable. We implement the same trick from the last test of the listenToMessages() service test using a flag to ensure we do not receive the message.

We can run this test with the following command:

```
mocha tests/block_user_test.js
```

When we run this code, we will see the following printed in our console:

![](bbox=[0.15, 0.27, 0.91, 0.51])

Again, our test failed as expected as we have not implemented our service yet.

# Service implementation

Now its time to implement our service to block messages from a given user. To do this, change the blockUser() service in the file services.js to use this implementation:

```
blockUser(payload,observable){
    let blocked = payload.blocked;
    observable = observable || this.listenToMessages(payload);
    return observable.filter(function(message){
        return message.from !== blocked;
    });
}
```