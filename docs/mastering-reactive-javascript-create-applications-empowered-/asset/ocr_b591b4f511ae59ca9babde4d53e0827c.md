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