
发生在上游请求期间。这个特殊目的类可用于不产生任何结果的后台任务：

```java
public abstract class AsyncJob
extends AsyncTask<Void, Void, Result<Void>> {
	@Override
	protected Result<Void> doInBackground(Void ...args) {
		Result<Void> result = new Result<Void>();
		try { runOnBackground(); }
		catch (Throwable e) { result.error = e; }
		return result;
	}
	@Override
	protected void onPostExecute(Result<Void> result) {
		if (result.error != null) { onFailure(result.error); }
		else { onSuccess(); }
	}
	// Backgroung Execution Task
	abstract void runOnBackground() throws Exception;
	// Error Callback
	abstract void onFailure(Throwable e);
	// Success Function
	abstract void onSuccess();
}
```

使用 AsyncJob，我们声明了三个抽象方法，任何 AsyncJob 子类都应提供实现。runOnBackground 应实现后台任务，onFailure 应用于处理执行异常，而 onSuccess 回调用于通知开发者作业已成功完成。

现在我们准备好实现 OnClickListener 了，它将构建一条上游消息并在后台将其分发到我们的 XMPP 服务器：

```java
OnClickListener sendListener = new OnClickListener() {
	@Override
	public void onClick(View v) {
		TextView msgText = (TextView) findViewById(R.id.msg);
		final String msgToSend = msgText.getText().toString();
		msgText.setText("");
		
		new AsyncJob() {
			// ...
			@Override
			void runOnBackground() throws Exception {
			    // Build the data Bundle wit our key/value pairs
			    Bundle data = new Bundle();
			    data.putString(USERNAME_KEY, "Helder");
			    data.putString(EXT_KEY, msgToSend);
			    data.putString("topic", NotificationGCMHandler.FORUM_TOPIC);

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
		}.execute();
	}
};
```

在我们的示例中，我们创建了一个 Bundle 对象，其中包含要分发的所有有效载荷数据。除此之外，我们还使用 java.util.Random.nextInt 实例方法创建了一个唯一的消息 ID。

此方法的参数包括遵循 <SENDER_ID>@gcm.googleapis.com 格式的地址、从随机整数生成的唯一消息 ID 字符串，以及包含有效载荷的 bundle。

一旦我们调用 GoogleCloudMessaging.send，如果可用活动连接，新的上游消息将立即发送，否则消息将被排队。一旦连接重新建立，排队的消息将被分发到 GCM 服务器。

## 注意

如果客户端在达到 20 条消息限制后尝试发送更多消息，它将返回一个错误。

GoogleCloudMessaging API 将以高效的方式重用和管理到 GCM 平台的连接，对我们来说透明地最大化设备电池寿命。

一旦消息被我们的 XMPP 服务器接收，消息将被分发到 topics/forum，因此，它将使用我们输入的消息更新 UI 消息流。

## GcmListenerService 传递回调

在某些情况下，当由于缺乏网络连接而无法与 GCM 服务器建立连接时，消息可能会在本地队列中保留很长一段时间。因此，为了丢弃在指定时间内未发送到 GCM 服务且仍留在队列中的消息，GoogleCloudMessaging API 提供了一个额外的 send 方法，该方法可以接收一个 TTL（Time to Live，生存时间）来设置消息的过期时间：

```java
void send (String to, String msgId, long timeToLive, Bundle data)
```

当你只有在消息在特定时间内到达时才相关时，这非常有效。当生存时间为 0 时，如果未连接，我们将尝试发送并立即返回错误。此情况不适用于我们的示例，因此我们将保留原始代码，使用不丢弃旧未发送消息的 send 方法。

重要的是要了解，当长时间无法连接到 GCM 平台时，应用程序 GCM 客户端只能排队最多 20 条消息。

除了上游过期功能之外，GcmListenerService 还允许我们通过重写 onMessageSent 和 onSendError 回调来接收上游消息的分发状态：

```java
void onMessageSent(String msgId)
void onSendError(String msgId, String error)
```

当消息被传递到 GCM 时调用 onMessageSent 回调，而在将消息分发到 GCM 连接服务器时出现错误时调用 onSendError。请注意，这两个回调都以消息标识符作为参数调用，因此您应该使用此标识符来定位已发送或发送失败的消息。

出于效率考虑，GCM 消息投递报告是成批传递的，因此不要指望在上传单条消息后立即收到回调执行。

为了在我们的聊天示例中接收上游消息的分发状态，我们将更新 NotificationGCMHandler 并重写 onMessageSent 和 onSendError：

```java
public class NotificationGCMHandler extends GcmListenerService {
    ...

    @Override
    public void onMessageSent(String msgId) {
        super.onMessageSent(msgId);
        Log.i(TAG, "Message w/ id="+msgId+" send to GCM Server ");
    }

    @Override
    public void onSendError(String msgId, String error) {
        super.onSendError(msgId, error);
        Log.e(TAG, "Message w/ id=" + msgId +
            " send failed with error "+error);
    }
}
```

在我们的 GcmListenerService 中定义的回调方法会将已发送或失败的消息打印到应用程序日志输出中。如果达到消息过期时间或上游排队消息数量达到最大值，消息分发可能会失败。

太好了！我们已完成基于 GCM 平台的群聊。在我们的旅程中，我们学习了如何使用一种电池高效的 API 来发送主题消息的上行和下行，该 API 可与 Google 服务器保持网络连接。该 API 允许我们在服务器与设备之间，或在一组设备之间创建双向通信通道。

## 使用 GCM Network Manager 执行任务

除了消息传递框架之外，GCM 库还附带 gcmNetworkManager，这是一个 API，允许我们在所有运行 API 级别 9（Android 2.1）及更高版本的设备上高效调度重复或周期性任务。对于运行 API 级别 21（Lollipop）及更高版本的设备，GCM Network Manager 内部使用原生 JobScheduler API，该 API 在第 4 章“探索 JobScheduler API”中详细介绍。与 JobScheduler API 类似，它将尝试批处理作业并减少从空闲状态唤醒的次数，以改善用户设备的电池寿命。

此外，使用 GCM Network Manager，我们还可以设置启动作业执行应满足的条件，例如设备处于充电状态或可用无计费 WiFi 连接。虽然 GCM API 为我们提供了与 JobScheduler API 相同的条件，但它可以在安装了 Google Play 服务的旧设备和新设备上使用。

因此，在尝试使用它之前，您需要使用 GoogleApiAvailability 类确保设备上可用 Google Play 服务版本，就像我们在 GCM 示例中所做的那样。

要了解更完整的解决方案，请查看我们之前示例中的 checkPlayServices() 函数。当需要用户操作来更新或安装 Google Play 服务时，前一个函数会显示对话框。

我们可以在 GCM Network Manager 上调度任务执行，在某些条件下运行，例如：

- 当某些网络连接可用时（任何网络可用，无计费网络连接）
- 当设备插入充电器时
- 在未来的预定义时间窗口内运行的任务
- 指定任务在重启后仍运行

虽然支持的条件与先前介绍的 Scheduler API 相同，并且适用于运行 Android Lollipop 的设备，但此 API
