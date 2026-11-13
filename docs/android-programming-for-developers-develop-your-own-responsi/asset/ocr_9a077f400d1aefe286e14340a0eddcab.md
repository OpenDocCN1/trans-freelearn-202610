All that remains is to display our group chat messages on the UI and implement the BroadcastReceiver that receives the broadcast Intents and updates the UI with the message username and text.

To process the Broadcast Intent, we must override the onReceive method of BroadcastReceiver in order to receive local Intent:

```
BroadcastReceiver onMessageReceiver = new BroadcastReceiver(){

@Override
public void onReceive(Context context, Intent intent) {

    TextView chatText = (TextView)findViewById(R.id.chatWindow);
    String username = intent.getStringExtra("username");
    String bodyText = intent.getStringExtra("text");
    String line = String.format("%s : %s%n", username,bodyText)
    // Prepend the message
    chatText.setText( line + chatText.getText().toString()));
}

};
```

Now, if you submit a topic message to the GCM using the following curl command, you will see a new message popping up on the UI TextView:

```
curl --request POST \
--url "https://gcm-http.googleapis.com/gcm/send" \
--header 'authorization: <SERVER_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "data": {
        "username": "heldervasc",
        "text": "Welcome to Asynchronous Android group chat"
    },
    "to": "/topicsforum"
}'
```