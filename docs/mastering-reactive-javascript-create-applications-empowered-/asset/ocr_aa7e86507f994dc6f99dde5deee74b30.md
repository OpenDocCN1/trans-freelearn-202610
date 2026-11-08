It receives only one mandatory parameter:

+   - event: It is a string containing the event name of incoming messages from the server

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
```