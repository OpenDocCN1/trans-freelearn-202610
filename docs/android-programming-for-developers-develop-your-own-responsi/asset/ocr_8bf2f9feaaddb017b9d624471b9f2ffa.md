combination of inheritance and composition to implement the default behavior in a Handler subclass and then mix in special behavior by passing in an instance of Handler.Callback.

In the aforementioned code, we use the composition to process the SAY_HELLO message and the inheritance to process the SAY_YES message:

```
// will be handled by SpeakHandler Handler
handler.sendEmptyMessage(SAY_HELLO);
// will be handled by Speaker Handler.Callback
handler.sendEmptyMessage(SAY_YES);
```

# Note

**Inheritance** should only be used when the relationship between a subclass and the superclass is permanent and strong and can't be decoupled. On the other hand, **composition** offers more flexibility for enhancements and testing.