

@Override
protected void onHandleIntent(Intent intent) {
    ...
    // Retrieve the token
    String token = instanceID.getToken(senderId,
            GoogleCloudMessaging.INSTANCE_ID_SCOPE,null);
    ...
    // Subscribe to Topics
    subscribeTopics(token);
}

private void subscribeTopics(String token) {
    GcmPubSub pubSub = GcmPubSub.getInstance(this);
    try {
        pubSub.subscribe(token, "topics " + TOPIC_NAME, null);
    } catch (Exception e) {
        Log.e(TAG, "Failed to subscribe to " + TOPIC_NAME, e);
    }
    ...
}

要从 GCM "forum" 主题取消设备订阅，可以使用注册令牌和主题名调用 GcmPubSub 的 unsubscribe() 方法。

主题消息的传递方式与我们之前示例中推送通知 GCM 消息的传递方式相同，都会送达我们的 GcmListenerService (NotificationGCMHandler)。主题消息会送达我们的应用，其中 from 字段存储主题名 topicsforum。

我将向您展示我们主题的典型主题消息可能是什么样子：

```
{
    "to": "topicsforum",
    "data": {
        "username": "heldervasc",
        "text": "I need to learn more about Android Development"
    }
}
```

data 对象字段是消息中我们可用于向应用传递自定义属性的字段。在我们的示例中，它承载了用户编写的用户名和文本信息。

接下来，考虑到 NotificationGCMHandler 将接收从 GCM 发送的主题消息，我们将对其进行更新以处理接收到的主题消息，并将每条主题消息广播给任何本地 BroadcastReceiver。

我们的 NotificationGCMHandler 只需将主题消息封装在 Intent 中，并将其分发到进程内的本地 Activity。这种异步通信技术已在前面章节中解释过，速度更快且更安全，因为您的消息不会离开应用：

```
public class NotificationGCMHandler extends GcmListenerService {

    public static final String FORUM_TOPIC = "topicsforum";
    public static final String USERNAME_KEY = "username";
    public static final String TEXT_KEY = "text";
    public static final String MSG_DELIVERY = "asyncforum";

    @Override
    public void onMessageReceived(String from, Bundle data) {

        // Verify if it is a forum message
        if (from.equals(FORUM_TOPIC)) {

            // Build an intent from the forum topic message.
            Intent intent = new Intent(MSG_DELIVERY);
            intent.putExtra(USERNAME_KEY, data.getString(USERNAME_KEY));
            intent.putExtra(TEXT_KEY, data.getString(TEXT_KEY));

            // Broadcast the intent to local interested objects
            LocalBroadcastManager.
                    getInstance(this).sendBroadcast(intent);
        } else ... {
            ...
        }
    }}

```

在我们的 GcmListenerService 转发从消息主题接收的消息后，是时候构建用于显示接收到的消息，并使用 GCM 上游消息向群聊发布消息的 Activity 了。

从前一章的工作基础出发，我们将创建一个 MessagingActivity，它还会验证 Google Play Services 是否可用，并在没有注册令牌时启动 RegistrationIntentService：

```
public class MessagingActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.chat_layout);
        if (checkPlayServices()) {
            ...
        }
    }
}
```

为了在 Activity 中接收和显示主题消息，我们将创建一个匿名 BroadcastReceiver 子类，用于动态注册和注销动作为 MSG_DELIVERY 的本地 Intent 接收。

由于我们只希望在 Activity 处于前台时接收主题消息，因此我们将在 onResume 和 onPause 回调中注册和注销本地广播：

```
public class MessagingActivity extends Activity {
    ...
    @Override
    protected void onResume() {
        super.onResume();
        // Create an intent filter to receive forum Intents
        IntentFilter filter = new IntentFilter(
                NotificationGCMHandler.MSG_DELIVERY);
        // Register the local Receive to receive the Intents
        LocalBroadcastManager.getInstance(this).
                registerReceiver(onMessageReceiver, filter);
    }

    @Override
    protected void onPause() {
        super.onPause();
        // Unregister the Local Receiver
        LocalBroadcastManager.getInstance(this).
                unregisterReceiver(onMessageReceiver);
    }
}
```

剩下的就是在 UI 上显示我们的群聊消息，并实现接收广播 Intent 并使用消息用户名和文本更新 UI 的 BroadcastReceiver。

为了处理广播 Intent，我们必须重写 BroadcastReceiver 的 onReceive 方法，以便接收本地 Intent：

```
BroadcastReceiver onMessageReceiver = new BroadcastReceiver(){

@Override
public void onReceive(Context context, Intent intent) {

    TextView chatText = (TextView)findViewById(R.id.chatWindow);
    String username = intent.getStringExtra("username");
    String bodyText = intent.getStringExtra("text");
    String line = String.format("%s : %s%n", username, bodyText);
    // Prepend the message
    chatText.setText(line + chatText.getText().toString());
}

};
```

现在，如果使用以下 curl 命令向 GCM 提交主题消息，您将看到一条新消息弹出在 UI TextView 上：

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

## 发送上游消息

虽然我们能够接收聊天群组消息，但无法从应用与消息流进行交互。此外，要使用 GCM 平台发送和处理上游消息，需要一个实现 XMPP 连接服务器协议的应用服务器来连接 GCM 服务器并接收上游 XMPP 消息。

为了处理我们的群组消息，我们构建了一个非常基础的 XMPP 服务器，用于处理来自设备的上游消息并将消息转发到主题消息。

基础 XMPP 服务器源代码可在 Packt Publishing 网站上获取。从 Packt 网站下载它，并在运行之前，使用您的 SenderID 和 ServerKey 更新 GCMServer.java 类文件中的静态字段。

```
private static final String SENDER_ID = "<YOUR_SENDER_ID>";
private static final String SERVER_KEY = "<SERVER_KEY>";
```

服务器将连接到 GCM 平台，启动一个 XMPP 会话，并处理所有发送到 <SENDER_ID>@gcm.googleapis.com 的消息。

要生成上游消息，我们在 UI 上创建了一个 EditText，并创建了一个按钮，触发后将发送上游消息。要在 GCM 平台上发送上游消息，应用需要提供以下字段：

- 我们在 GCM 平台上的服务器地址：
  <SENDER_ID>@gcm.googleapis.com。
- 唯一消息标识符 (message_id)
- 带有自定义键/值对的消息负载

现在，让我们更新 MessagingActivity 以基于 EditText 输入字段发送上游消息。由于上游分发需要网络访问，而且正如您所知，我们不能在主线程上执行网络操作，因此必须使用 AsyncTask 子类在主线程之外执行。在 Activity 类中，我们实现了一个名为 AsyncJob 的基本异步构造，以便在后台执行网络操作，捕获任何异常

