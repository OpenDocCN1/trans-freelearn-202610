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