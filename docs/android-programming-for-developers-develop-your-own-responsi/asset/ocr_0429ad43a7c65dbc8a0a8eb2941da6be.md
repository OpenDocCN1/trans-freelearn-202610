```
@Override
void runOnBackground() throws Exception {
    // Build the data Bundle wit our key/value pairs
    Bundle data = new Bundle();
    data.putString(USERNAME_KEY, "Helder");
    data.putString(EXT_KEY, msgToSend);
    data.putString("topic", NotificationGCMHandler.
            FORUM_TOPIC);

    // Generate a random message Id
    String id = Integer.toString(new Random().nextInt());

    // Get the GCMMessaging instance
    GoogleCloudMessaging gcm = GoogleCloudMessaging.
            getInstance(MessagingActivity.this);

    // Sends the Message to the GCM platform
    gcm.send(getString(R.string.gcm_SenderId) +
            "@gcm.googleapis.com", id, data);
}
@Override
void onFailure(Throwable e) {
    //... Handle the exception
    Log.e(TAG,"Failed to send upstream message to forum",e);
}
@Override
void onSuccess() {
    //.. No Exception thrown
}
} }.execute();
};
```

In our example, we created a Bundle object with all the payload data that we want to dispatch. Beyond that, we created a unique message ID using the java.util.Random.nextInt instance method.

This message receives as parameters the address following the format <SENDER_ID>@gcm.googleapis.com, the unique message ID string generated from the random integer, and the bundle with your payload.

Once we invoke the GoogleCloudMessaging.send, if an active connection is available, the new upstream message will be sent immediately, otherwise the message will be queued. Once the connectivity is re-established, the queued