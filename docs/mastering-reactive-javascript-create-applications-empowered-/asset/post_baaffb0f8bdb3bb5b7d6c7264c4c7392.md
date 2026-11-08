
It receives only one mandatory parameter:

- event: It is a string containing the event name of incoming messages from the server

It will have the following implementation:

```
function(event){
    var eventObservable = Rx.Observable.fromEvent(socket,event);
    return connectionObservable
        .flatMap(eventObservable)
        .takeUntil(disconnectObservable);
}
```

In this function, we first create an Observable from the given event name in the socket, and we apply the same trick that we used in the previous method to listen to data only while we are connected, starting with the connectionObservable and stopping with the disconnectObservable.

These two methods are the only thing we need to start sending and receiving WebSocket messages.

If you followed the instructions correctly, you will end up with a file as follows:

```
var Rx = require('rx');
var io = require('socket.io-client');
var socket = io('http://localhost:3000');

var connectionObservable = Rx.Observable.fromEvent(socket,'connect');
var disconnectObservable = Rx.Observable.fromEvent(socket,'disconnect');

module.exports = {
    addSender:function(observable){
        connectionObservable
            .flatMap(observable)
            .takeUntil(disconnectObservable)
            .map(function(data){
                return JSON.stringify(data);
            })
            .subscribe(function(data){
                socket.emit('request',data);
            });
    },
    listen:function(event){
        var eventObservable = Rx.Observable.fromEvent(socket,event);
        return connectionObservable
            .flatMap(eventObservable)
            .takeUntil(disconnectObservable);
}
};
```

## Using RxJS to manage user input

Now we must implement the observables to listen to user input; they are going to be used to send messages to the server.

Let's implement the events.js file inside the client folder.

The first thing to do is to use the RxJS module:

```
var Rx = require('rx');
```

We want to send the user messages every time the user hits the Send message button or when the user hits enter in the message box.

For this reason, we need two observables, the first to trigger when the user clicks on the Send message button:

```
var sendPressedObservable = Rx.Observable
.fromEvent(document.getElementById('send_message'),'click');
```

This observable is simple and just propagates the event whenever the user clicks on the button.

The second observable will propagate the event whenever the user hits the Enter key in the message box:

```
var enterPressedObservable = Rx.Observable
.fromEvent(document.getElementById('message_input'),'keypress')
.filter(function(event){
    var ENTER_KEY_CODE=13;
    return event.keyCode===ENTER_KEY_CODE ||
    event.which===ENTER_KEY_CODE;
});
```

This observable listens to all events of keys being pressed in the message box; we then filter to propagate only when the key pressed is the Enter key.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_283_0.png)

The number 13 represents the code for the Enter key; we compare it with the properties keyCode and which from the variable event for browser compatibility.

Now we must merge both observables to create an observable that propagates when the Enter key is pressed or when the user hits the **Send message** button.

We can do this with this code:

```
var messageSubmitObservable = sendPressedObservable
.merge(enterPressedObservable)
```

When the user hits this button, he wants to propagate the content of the message box, and we can implement this with the map() operator:

```
.map(function() {
  return document.getElementById('message_input').value;
})
```

There is no reason to propagate data if the value is empty, so we also add a filter to check if it has something to send:

```
.filter(function(message) {
  return message != "";
})
```

And for the last, we want to erase the content of the message box when the user sends the message, so we need to add a side effect with the do() operator:

```
.do(function() {
  document.getElementById('message_input').value='';
})
```

Now we have an observable that emits data every time a user wants to send a message; this observable has the following code:

```
var messageSubmitObservable = sendPressedObservable
  .merge(enterPressedObservable)
  .map(function() {
    return document.getElementById('message_input').value;
  }).filter(function(message) {
    return message != "";
  }).do(function() {
    document.getElementById('message_input').value='';
  });
```

In this file we wanted to map all possible user inputs, so the last thing we need to do is to export this observable, so another file can map the messages and send it to the server:

```
module.exports=messageSubmitObservable;
```

If you followed the instructions correctly, your events.js file on the client folder will have the following content:

```
var Rx = require('rx');

var sendPressedObservable = Rx.Observable
    .fromEvent(document.getElementById('send_message'),'click');

var enterPressedObservable = Rx.Observable
    .fromEvent(document.getElementById('message_input'),'keypress')
    .filter(function(event){
        var ENTER_KEY_CODE=13;
        return event.keyCode===ENTER_KEY_CODE ||
        event.which===ENTER_KEY_CODE;
    });

var messageSubmitObservable = sendPressedObservable
    .merge(enterPressedObservable)
    .map(function(){
        return document.getElementById('message_input').value;
    }).filter(function(message){
        return message !="";
    }).do(function(){
        document.getElementById('message_input').value='';
    });
module.exports=messageSubmitObservable;
```

Now we must implement a way to send the user messages to the server.

## Connecting user interactions and server communication

Now that we have already implemented the observable to listen to user interactions and also a way to communicate with the server, we must now implement three things:

- A way for the user to provide their name
- Send messages to the server when a user wishes
- Show on the screen the incoming messages from the server

To implement this last part of our application that connects the user interactions with the server, we are going to implement the index.js file located in the client folder.

The first thing to implement this is to require the modules we need:

```
var Rx = require('rx');
var connection = require('./connection');
var events = require('./events');
```

With these three lines of code we require the RxJS module and the connection and events modules. Now we can ask a user for their name, and we will use a prompt for this:

```
var logged = prompt("Please enter your name", "");
```

Now when you run the application and open it in the browser, you will be prompted for your name, as you can see in this screenshot:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_286_0.png)

The name you provide is going to be used in the messages.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_286_1.png)

To keep the example as simple as possible, we decided to not create any kind of authentication.

Now we need to create the observable of messages for the user.

This can be done with this code:

```
var messagesObservable = events.map(function(value){
    var userName,splittedUserInput,action,content;
    if(value.indexOf('!')===0){
        userName = value.substring(1);
        return {service:'blockUser',blocked:userName,from:logged};
    }
    if(value.indexOf('/')===0){
        splittedUserInput = value.split(' ');
        action = splittedUserInput[0].substring(1);
        content = splittedUserInput.slice(1).join(' ');
        return {
            service:'command',
            action:action,
            content:content,
            from:logged
        };
    }
    if(value.indexOf('@')===0){
        splittedUserInput = value.split(' ');
        userName = splittedUserInput[0].substring(1);
        content = splittedUserInput.slice(1).join(' ');
        return {
            service:'sendMessage',
            to:userName,
            content:content,
            from:logged
        };
    }
    return {service:'sendMessage',content:value,from:logged};
}).merge(
    Rx.Observable.of({service:'listenToMessages',me:logged})
);
```

Here we need to map the user input to the format of messages expected by the server, and we will do this using the map() operator; the message can follow one of four different patterns to call different services:

- If the message starts with an exclamation mark (!), it is a message to block a user, and the exclamation mark must be followed by the username; we handle this type of message on the first if() clause in the function
- If the message starts with a slash (/), it is a command message and must be followed by the command; on our server we support two different commands, echo and roll_a_dice; this is handled by the second if() clause
