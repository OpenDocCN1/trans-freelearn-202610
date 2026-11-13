
compromised, such as with a suspicious use of the token. This procedure is usually initiated by the instanceID provider.

Instance ID API is used to manage security tokens that authorize your application or your server to interact with the GCM Service.

![](img/35d01831c19688a44e50bce7c489007a_1127_0.png)

Beyond the creation of new tokens, the InstanceID singleton instance is able to delete tokens or even invalidate an InstanceID.

```
void deleteInstanceID()
void deleteToken(String authorizedEntity, String scope)
```

## Receiving downstream messages

With the basic blocks required to set up the GCM client already in place, in our first GCM example we will send a simple downstream message through the GCM Platform and print it as a notification on the Android Notification drawer.

To handle GCM messages, we will have to implement a service that extends from GcmListenerService and override the onMessageReceived(String,Bundle) method. Since GcmReceiver extends WakefulBroadcastReceiver, it is guaranteed that the CPU is going to be awake until the service completes the delivery.

Our GcmListenerService subclass will receive a message from GCM and create an Android Notification as soon as it receives it.

```
public class NotificationGCMHandler extends GcmListenerService {
public static final int NOTIFICATION_ID ="GCMNotification".
hashCode();

@Override
public void onMessageReceived(String from, Bundle data) {

String msgType = data.getString("type");

// Notification Message received from GCM.
if ( msgType.startsWith("my_notifications") ) {
createNotification(data.getString("title"),
data.getString("body"));
}
}
private void createNotification(String title, String body) {
// Elided for brevity...
}
}
```

We also need to register our GcmListenerService service class in the AndroidManifest.xml registering the service to receive the com.google.android.c2dm.intent.RECEIVE action:

```
<service android:name=".chapter10.NotificationGCMHandler"
android:exported="false" >
<intent-filter>
    <action
    android:name="com.google.android.c2dm.intent.RECEIVE">
</intent-filter>
</service>
```

To instigate the initial registration with GCM, we will create an Activity that will start the RegistrationService's IntentService to retrieve the required token. However, before we try to retrieve the token, we will have to check if the Google Play Services is available on the device, and that the version installed on this device is no older than the one required by this client.

Let's get started by implementing the Activity.onCreate method, triggering interaction with the GCM platform:

```
public class MyChatActivity extends Activity {
    public static final String SENT_TOKEN_TO_SERVER = "sent2Server";
    private final static int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    if (checkPlayServices()) {
    Log.i(LOG_TAG, "Registering to GCM");
    SharedPreferences sharedPref = PreferenceManager.
    getDefaultSharedPreferences(this);
    // Registering is started when there is no available token
    boolean sentToken = sharedPref.
    getBoolean(SENT_TOKEN_TO_SERVER, false);
    if (!sentToken) {
    //....Print an error
    }
    Intent int = new Intent(this, RegistrationIntentService.class);
    startService(int);
    }
    }
}
```

Before we start the registration service, checkPlayServices will verify if the Google Play Services is installed on the device. If the service is not available, a dialog box is shown to the user that allows the users to download it from the Play Store or enable it on the device system settings:

```
private boolean checkPlayServices() {
// Returns the singleton instance of GoogleApiAvailability.
GoogleApiAvailability apiAvailability = GoogleApiAvailability.
        getInstance();
// Verify if the Google Play Service installed is
// installed and compatible with GCM Library used
int rc = apiAvailability.isGooglePlayServicesAvailable(this);
if ( rc != ConnectionResult.SUCCESS ) {

    // The error can be resolved with a user action
    if (apiAvailability.isUserResolvableError(rc)) {
        // Shows a user action dialog to resolve the issue
        apiAvailability.getErrorDialog(this,
                rc, PLAY_SERVICES_RESOLUTION_REQUEST).show();
    } else {
        Log.i(TAG, "This device is not supported.");
        // Finishing the Activity
        finish();
    }
    return false;
}
return true;
}
```

When the isGooglePlayServicesAvailable returns success, we return true from the function and initiate the registration service.

When the function returns an error that can be resolved by the user, such as SERVICE_VERSION_UPDATE_REQUIRED, a localized dialog is shown to the user to correct the problem. The dialog could redirect the user to the Play Store if Google Play Services is out of date or missing, or to system settings if Google Play Services is disabled on the device.

If the returned error cannot be solved by a user action, we simply finish the current Activity and print a log message because the device will not be able to register in GCM and receive the downstream message.

Yes! We finished the application GCM bootstrap, and as soon as we start the Activity and register with GCM, the device will be ready to receive downstream messages from GCM.

Remember that our registration service will print a registration token to the log output, so don't forget to note it when you run the MyChatActivity for the first time.

+   I ....: GCM Application Instance Identifier: <InstanceId>
+   I ....: GCM Registration Token: <Registration Token>

To interact with GCM you could set up an HTTP or XMPP backend server that uses the server credentials to connect to the GCM Service. For simplicity and testing we will build and submit HTTP messages directly.

To send a downstream message to our device we will have to send HTTP POST a message with a JSON object in the payload, setting the `to` field to our noted registration token and a data object field with our custom notification properties: title and body and type.

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

To submit HTTP messages to the GCM platform, you can use the curl command or use the Chrome web application Postman (http://www.getpostman.com/). Here is the curl command that will submit the previous message to GCM:

```
$ curl --request POST \
--url https://gcm-http.googleapis.com/gcm/send \
--header 'authorization: key=<Server API Key>' \
--header 'Content-Type: application/json' \
--data '{"data":{"title":"Hello from GCM","body":"Hello from your fake server","type":"notification"},"to":"<DeviceRegistrationToken>"}'
```

Don't forget to replace the <Server API Key> with the API key generated on the Google Cloud Console registration and replace <DeviceRegistrationToken> with the token generated for your device. Notice that the downstream data messages have a maximum 4KB payload.

If everything goes well with your GCM setup, your data object properties are passed to your onMessageReceived() method in the data bundle object and the GCM service will send you back a HTTP Response (200) with a message body similar to the one below:

```
{
"multicast_id": 6425212369847183592,
"success": 1,
"failure": 0,
"canonical_ids": 0,
"results": [{
"message_id": "0:1456441876781708%69ee9872f9fd7ecd"
}]
}
```

## Receiving messages from topic

The downstream messages allow us to send short (4KB) messages to alert the user of new updates, new content or even reminders.

A downstream message is a one-way communication channel where the users can receive messages, but they cannot respond to them directly or take any immediate action.

To build interactive experiences, such as a chat system, we will have to support a bidirectional communication where the user can receive downstream messages and also send upstream messages to other devices or groups of devices.

In our next example, we will build a simple group messaging system based on the GCM upstream messaging and topic messaging features. The group messaging system will allow multiple devices to publish text messages to a shared message channel.

GCM topic messaging allows your backend server to send a message to devices that have a particular topic. Once the GCM receives a message to a particular topic, it will route and deliver the message to the subscribed devices transparently using the list of subscribed devices managed on the GCM platform.

A topic is identified by the name that follows the next regular expression:

```
topics[a-zA-Z0-9_-.~%]+
```

To start receiving messages related to a particular topic name, a GCM-registered client application will have to subscribe in GCM with its own registration token and the desired topic stream.

First of all, we will update our RegistrationIntentService to subscribe our application to the "topicsforum" message stream using the registration token received:

```
public class RegistrationIntentService extends IntentService {
    private static final String TOPIC_NAME = "forum";
}
```
