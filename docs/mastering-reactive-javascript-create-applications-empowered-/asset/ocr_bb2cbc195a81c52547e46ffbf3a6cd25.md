It can be done with the following code:

```
Rx.Observable
.fromEvent(document.getElementById('message_input'),'click')
.bufferWithTimeOrCount(800,3)
.filter(function(events){
  return events.length === 3;
})
```

That's it; this observable triggers an event every time a user does a triple click action in the message box.

Now we need to change the value of the message box with the /roll_a_dice command, and we can do this with a subscription in this Observable:

```
Rx.Observable
.fromEvent(document.getElementById('message_input'),'click')
.bufferWithTimeOrCount(800,3)
.filter(function(events){
  return events.length === 3;
}).subscribe(function(){
  document.getElementById('message_input').value='/roll_a_dice';
});
```

If you run this code and test it in your browser, you will see the message box being filled with the /roll_a_dice command every time you do a triple click in it:

![](bbox=[0.09, 0.55, 0.96, 0.83])