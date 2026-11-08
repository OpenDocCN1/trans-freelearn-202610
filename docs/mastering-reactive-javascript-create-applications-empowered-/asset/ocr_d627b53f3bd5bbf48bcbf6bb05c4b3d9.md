```
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

connection.addSender(messagesObservable);

connection.listen('message')
.map(function(message){
    var li = document.createElement("li");
    li.innerText = message.from + ' says to '+
    (message.to || 'everybody')+' : '+message.content;
    return li;
})
.subscribe(function(node){
    document.getElementById("messages").appendChild(node);
});
```

Congratulations! This means you have your first application using functional reactive programming with RxJS. You already can send and receive messages using it.