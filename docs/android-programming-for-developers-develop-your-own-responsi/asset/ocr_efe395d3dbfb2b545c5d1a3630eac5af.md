output, so don't forget to note it when you run the MyChatActivity for the first time.

+   I ....: GCM Application Instance Identifier: <InstanceId>
+   I ....: GCM Registration Token: <Registration Token>

To interact with GCM you could set up an HTTP or XMPP backend server that uses the server credentials to connect to the GCM Service. For simplicity and testing we will build and submit HTTP messages directly.

To send a downstream message to our device we will have to send HTTP POST a message with a JSON object in the payload, setting the field to to with our noted registration token and a data object field with our custom notification properties: title and body and type.

Here is a JSON-formatted message that will generate an Android notification as soon as NotificationGCMHandler receives it from GCM:

```
{
  "data": {
    "title": "Hello from GCM",
    "body": "Hello from your fake server",
    "type": "my_notifications"
  },
  "to": "<DeviceRegistrationToken>"
}
```

To submit HTTP messages to the GCM platform, you can use the curl command or use the chrome web application Postman (http://www.getpostman.com/). Here is the curl command that will submit the previous message to GCM:

```
$ curl --request POST \
--url https://gcm-http.googleapis.com/gcm/send \
--header 'authorization: key=<Server API Key>' \
--header 'Content-Type: application/json' \
--data '{"data":{"title":"Hello from GCM","body":"Hello from your fake server","type":"notification"},"to":"<DeviceRegistrationToken>"}'
```

Don't forget to replace the <Server API Key> with the API key generated on the Google Cloud Console registration and replace <DeviceRegistrationToken> with the token generated for your device. Notice