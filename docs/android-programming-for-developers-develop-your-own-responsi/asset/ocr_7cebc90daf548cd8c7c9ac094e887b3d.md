message sender wants:

```
public static class SpeakHandler extends Handler {
    public static final int SAY_HELLO = 0;
    public static final int SAY_BYE = 1;
    public static final int SAY_WORD = 2;
    @Override
    public void handleMessage(Message msg) {
        switch(msg.what) {
        case SAY_HELLO:
        sayWord("hello"); break;
        case SAY_BYE:
        sayWord("goodbye"); break;
        case SAY_WORD:
        // Get an Object
        sayWord((String)msg.obj); break;
        default:
        super.handleMessage(msg);
        }
    }
    private void sayWord(String word) { ... }
}
```

Within our handleMessage method, we can access the payload of the message
directly by accessing the public obj property. The Message payload can be set
easily via alternative static obtain methods:

```
Message msg = Message.obtain(handler,
SpeakHandler.SAY_WORD, "Welcome!");
handler.sendMessage(msg);
```

In the previous example, we basically create a message that the what property is
SAY_WORD and the obj property is Welcome!.

While it should be quite clear what this code is doing, you might be wondering
why we didn't create a new instance of Message by invoking its constructor and
instead invoked its static method, obtain.

The reason is efficiency. Messages are used only briefly—we instantiate,
dispatch, handle, and then discard them. So, if we create new instances each
time, we are creating work for the garbage collector.

Garbage collection is expensive, and the Android platform goes out of its way to