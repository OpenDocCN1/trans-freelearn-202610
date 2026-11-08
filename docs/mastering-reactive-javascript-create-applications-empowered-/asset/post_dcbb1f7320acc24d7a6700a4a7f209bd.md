
In this test, we are first running a setTimeout() to wait for the connection to be available (we are waiting 100 milliseconds, which should be enough), and then we fill the message box with a message to force a click of the send button. We wait again a few milliseconds for the message to be delivered and check the content of the <ul> element, to see if it contains the message. We finally end our tests, calling the done() function.

To test our application, we need to have everything running, so, after saving this file, start your application and open it in your browser on this address:
http://localhost:3000/index_test.html.

You can see your test passing in this screen:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_300_0.png)

We want to make our tests independent and, for this reason, we are going to add a beforeEach hook to clean up the HTML elements before each test:

```
beforeEach(function() {
    document.getElementById("message_input").value = "";
    document.getElementById("messages").innerHTML = "";
});
```

All the other tests follow the same pattern as the first test.

On the second test, we will check for private messages:

```
it('should send and receive private messages', function(done) {
    setTimeout(function(){
        document.getElementById("message_input").value = "@Erich hello";
        document.getElementById("send_message").click();
```

```
setTimeout(function(){
assert.equal(document.getElementById("messages").children.length,1);
assert.equal(
document.getElementById("messages").children[0].innerText,
"Erich says to Erich : hello"
);
done();
},500);
},100);
});
```

The only differences between this test and the first are the messages sent and received.

In the third test, we will see if the roll_a_dice command works:

```
it('should support roll_a_dice command', function(done) {
    setTimeout(function(){
        document.getElementById("message_input").value = "/roll_a_dice";
        document.getElementById("send_message").click();
        setTimeout(function(){
            assert.equal(document
                .getElementById("messages").children.length,1);
            assert.equal(
                /server says to Erich : Result is [1-6]/ .test(
                document.getElementById("messages")
                .children[0].innerText ),true );
            done();
        },500);
    },100);
});
```

Here we use a Regex to check the content of the file, as the number returned by the service can be any between 1 and 6.

The last test checks if the echo command works as expected:

```
it('should support echo command', function(done) {
    setTimeout(function(){
        document.getElementById("message_input").value = "/echo hello";
        document.getElementById("send_message").click();
        setTimeout(function(){
            assert.equal(document
                .getElementById("messages").children.length,1);
            assert.equal(document
                .getElementById("messages").children[0].innerText,
                "server says to Erich : hello"
            );
```

```
done ();
},500);
},100);
});
```

If you followed this correctly, you can run the tests in your browser and see a screen like this one:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_302_0.png)

The final code for your tests looks as follows:

```
<html>
<head>
    <meta charset="utf-8">
    <title>Tests</title>
    <link href="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.css" rel="stylesheet" />
</head>
<body>
<div id="mocha"></div>
<div style="display:none">
    <ul id="messages"></ul>
    <div class="message_container">
```

```
<input id="message_input"/><button id="send_message">Send message</button>
    </div>
</div>
<script src="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.js"></script>
<script
src="https://cdnjs.cloudflare.com/ajax/libs/chai/3.5.0/chai.min.js"></script>
<script>
    window.prompt=function(){
        return "Erich";
    };
</script>
<script src="/bundle.js"></script>
<script>
    mocha.setup('bdd');
    var assert = chai.assert;
    beforeEach(function(){
        document.getElementById("message_input").value = "";
        document.getElementById("messages").innerHTML = "";
    });
    describe('Web chat test', function() {
        it('should send and receive public messages', function(done) {
            setTimeout(function(){
                document.getElementById("message_input").value = "hello";
                document.getElementById("send_message").click();
                setTimeout(function(){
                    assert.equal(document
                        .getElementById("messages").children.length,1);
                    assert.equal(document
.getElementById("messages").children[0].innerText,
                        "Erich says to everybody : hello"
                    );
                    done();
                },500);
            },100);
        });
        it('should send and receive private messages', function(done) {
            setTimeout(function(){
                document.getElementById("message_input").value = "@Erich hello";
                document.getElementById("send_message").click();
                setTimeout(function(){
                    assert.equal(document
                        .getElementById("messages").children.length,1);
                    assert.equal(document
```

```
.getElementById("messages").children[0].innerText,
"Erich says to Erich : hello"
);
        done();
      },500);
    },100);
  });
  it('should support roll_a_dice command', function(done) {
    setTimeout(function(){
      document.getElementById("message_input").value =
"/roll_a_dice";
      document.getElementById("send_message").click();
      setTimeout(function(){
        assert.equal(document
          .getElementById("messages").children.length,1);
        assert.equal(
          /server says to Erich : Result is [1-6]/ .test(
            document.getElementById("messages")
            .children[0].innerText ), true );
        done();
      },500);
    },100);
  });
  it('should support echo command', function(done) {
    setTimeout(function(){
      document.getElementById("message_input").value =
        "/echo hello";
      document.getElementById("send_message").click();
      setTimeout(function(){
        assert.equal(document
.getElementById("messages").children.length,1);
        assert.equal(document
.getElementById("messages").children[0].innerText,
          "server says to Erich : hello"
        );
        done();
      },500);
    },100);
  });
});
  mocha.run();
</script>
</body>
</html>
```

Congratulations! With this test we finished our project using functional reactive programming Now you have mastered RxJS and are ready to use it in your own projects.

As a last exercise, you can try to improve the tests in this application or create new interactions with it.

## Summary

In this chapter, we created the last part of our project using functional reactive programming; here we used a lot of the knowledge from the other parts of this book, such as using functional reactive programming in the browser, creating observables from user inputs, using several different operators, modeling messages and connections as observables, subscribing to an observable, buffering data from an observable to process it in batch, and creating simple tests for the frontend application.

We did not use all of the knowledge we learned throughout this book, because it is hard to find a sample application which uses everything, and while keeping it simple enough to use as an example inside a book.

Keep in mind that this knowledge is important and can be used in other programs in your day to day work.

## Index

A
application
browser, testing 167
server, testing 164
testing, RxJS used 169
B
backpressure
about 95, 96, 190
dealing, common strategies 97
dealing, lossy strategies 97
dealing, with loss-less operators 107
Bacon.interval() 36
bacon.js observables
versus RxJS observables 49
bacon.js
about 47
cold observables 49
error, handling 33
hot observables 49
installing 11
observables 20
Observables, lazy evaluation 44
observables, modifying 28
observables, reusing 42
properties 27
used, for transforming events 36
browserify 256
buffering 108
bufferWithCount() operator 108
bufferWithTime() operator 112
bufferWithTimeOrCount() operator 114
C
callback hell 134
CasperJS 282
catch() operator 156, 161
client application
building 259, 261, 262, 263
structure 257, 259
testing 281, 284, 285, 286
cold observables 49, 106
CompositeDisposable 77
concat() operator
using 134
concatAll() operator
about 141
using 141
D
data
aggregating 189
filtering 187, 188, 189
DataSource
about 228, 229
Redis, using for messages 252
debounce() operator 102
dependencies
installing 256, 257
Disposable 49, 69
do() operator 199, 200
about 198
aliases 200
DOM
changes, batching 277, 279, 280
Don't Repeat Yourself (DRY) 133, 151
E
empty() method 62
error
application, testing 164
catch() operator 156
dealing 152
