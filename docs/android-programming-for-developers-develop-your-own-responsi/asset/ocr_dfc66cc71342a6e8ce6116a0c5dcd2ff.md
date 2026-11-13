```
handler.sendMessageDelayed(msg1, 000000),
handler.sendMessageDelayed(msg2, 600000);
```

```
// try to remove the messages
handler.removeMessages(SpeakHandler.SAY_WORD,
stringRef1);
handler.removeMessages(SpeakHandler.SAY_WORD,
new String("Welcome Home!"));
```

```
// Create a Print Writer to Process StandardOutput
PrintWriterPrinter out =
new PrintWriterPrinter(new PrintWriter(System.out,true));
```

```
// Dump the Looper State
handler.getLooper().dump(out,">> Looper Dump ");
```

As explained before, the second remove invocation will not remove the message
added previously, because the stringRef1 reference is different from the new
reference passed in, despite the string content being the same.

Here is the output from the looper dump, with the message that was not canceled
successfully:

```
>> Looper Dump Looper (main, tid 1) {a15844a}
>> Looper Dump Message 0: { when=+10m0s0ms
what=2
obj=Welcome Home! target=...SpeakHandler }
>> Looper Dump (Total messages: 1,
polling=false, quitting=false)
```