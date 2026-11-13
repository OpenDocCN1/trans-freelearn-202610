

handler.sendMessageDelayed(msg1, 000000),
handler.sendMessageDelayed(msg2, 600000);

```
// try to remove the messages
handler.removeMessages(SpeakHandler.SAY_WORD,
stringRef1);
handler.removeMessages(SpeakHandler.SAY_WORD,
new String("Welcome Home!"));
```

```
// Create a Print Writer to Process StandardOutput
PrintWriter out =
new PrintWriter(System.out, true);
```

```
// Dump the Looper State
handler.getLooper().dump(out,">> Looper Dump ");
```

如前所述，第二次 remove 调用不会删除之前添加的消息，因为 stringRef1 引用与传入的新引用不同，尽管字符串内容相同。

以下是 looper 转储的输出，其中包含未成功取消的消息：

```
>> Looper Dump Looper (main, tid 1) {a15844a}
>> Looper Dump Message 0: { when=+10m0s0ms
what=2
obj=Welcome Home! target=...SpeakHandler }
>> Looper Dump (Total messages: 1,
polling=false, quitting=false)
```

## 组合与继承

到目前为止，我们通过子类化 Handler 来重写其 handleMessage 方法，但这不是我们唯一的选择。我们可以在 handler 构造期间传递 Handler.Callback 的实例，以优先使用组合而非继承：

```
boolean handleMessage(Message msg)
```

让我们假设我们想在不更改原始 Handler 的情况下扩展我们的扬声器，并且我们想在 Handler.Callback 类上添加新操作：

```
public class Speaker implements Handler.Callback {
    public static final int SAY_WELCOME = 2;
    public static final int SAY_YES = 3;
    public static final int SAY_NO = 4;

    @Override
    public boolean handleMessage(Message msg) {
        switch(msg.what) {
            case SAY_WELCOME:
                sayWord("welcome"); break;
            case SAY_YES:
                sayWord("yes"); break;
            case SAY_NO:
                sayWord("no"); break;
            default:
                return false;
        }
        return true;
    }
    private void sayWord(String word) { }
}
```

请注意，这里 handleMessage 的签名略有不同——我们必须返回一个布尔值，指示是否处理了该 Message。要创建使用我们扩展的 Handler，我们只需在 handler 构造期间传递 Handler.Callback 实现即可：

```
Handler handler = new SpeakHandler(new Speaker());
```

如果我们从回调的 handleMessage 方法返回 false，Handler 将调用其自身的 handleMessage 方法，因此我们可以选择在 Handler 子类中实现默认行为，并通过传递 Handler.Callback 实例来混入特殊行为，从而结合使用继承和组合。

在上述代码中，我们使用组合来处理 SAY_HELLO 消息，并使用继承来处理 SAY_YES 消息：

```
// will be handled by SpeakHandler Handler
handler.sendEmptyMessage(SAY_HELLO);
// will be handled by Speaker Handler.Callback
handler.sendEmptyMessage(SAY_YES);
```

## 注意

**继承**仅在子类与超类之间的关系永久且强大且无法解耦时才应使用。另一方面，**组合**为增强和测试提供了更多灵活性。

## 使用 Handler 和 ThreadHandler 进行多线程

在典型的 Android 异步应用程序中，UI 线程将长时间计算操作交给后台线程，后台线程随后执行任务并将结果发布回主线程。

到目前为止，我们只是使用 Handler 向主线程发送消息，因此下一步自然是设计一个多线程场景，其中线程间通信由 Handler 结构管理。

让我们扩展之前的示例并创建一个天气预报检索器。

想象一下这个场景：当我们点击 UI 按钮时，主线程将要求后台线程检索天气预报，当收到天气预报响应时，后台线程将要求主线程显示接收到的天气预报。

我们将首先创建 WeatherRetriever，它负责接收天气预报请求，然后检索预报句子并将结果发布回 mainHandler 对象。

在场景组装期间，WeatherRetriever handler 通过第一个构造函数参数附加到后台 Looper 上，以便在远离主线程的单独执行线上运行。第二个构造函数参数用于设置 handler 以发布结果。

在 handleMessage 方法中，handler 能够处理当天的预报消息请求 (GET_TODAY_FORECAST) 或第二天的请求 (GET_TOMORROW_FORECAST)，最终调用长时间计算的 getForecast() 操作。

长时间计算的 getForecast() 可能会长时间阻塞线程执行，但这不再是问题，因为我们将在优先级较低的后台线程中运行它，这不会阻止 UI 及时渲染，从而防止发生 ANR 错误，并使应用程序对用户交互更加灵敏：

```
public class WeatherRetriever extends Handler {
    private final Handler mainHandler;
    public static final int GET_TODAY_FORECAST = 1;
    public WeatherRetriever(Looper looper, Handler mainHandler){
        super(looper);
        this.mainHandler = mainHandler;
    }
    // Long Computing Operation
    String getForecast(){ ... }
    @Override
    public void handleMessage(Message msg) {
        switch(msg.what) {
            case GET_TODAY_FORECAST:
            ...
            final String sentence = getForecast();
            Message resultMsg =
                mainHandler.obtainMessage(
                WeatherPresenter.TODAY_FORECAST, sentence);
                this.mainHandler.sendMessage(resultMsg);
                break;
            }
        }
};
```

其次，我们将构建 WeatherPresenter，它将处理来自后台操作的预报结果，并在主线程上向用户显示：

```
public class WeatherPresenter extends Handler {
public static final int TODAY_FORECAST = 1;
@Override
public void handleMessage(Message msg) {
switch(msg.what) {
case TODAY_FORECAST:
readTodayWeather((String) msg.obj); break;
... 
}
}
private void readTodayWeather(String word) {
// Present the weather forecast on the UI
...
}
```

我们之前在本章中详细介绍了一种使用 SimpleLooper 类设置 Looper 线程的方法，但有一种更简单的方法，即使用 SDK 专门为此目的提供的类：android.os.HandlerThread。

创建 HandlerThread 时，我们需要指定两件事：线程的名称（调试时很有帮助）以及其优先级，优先级必须从 android.os.Process 类的静态值集合中选择：

```
HandlerThread thread = new HandlerThread("background", Process.THREAD_PRIORITY_BACKGROUND);
```

## 提示

在 Android 中，线程优先级映射到 Linux nice 级别，后者决定了线程运行的频率。nice 值为 -20 是最高优先级，19 是最低优先级。默认 nice 级别为 0。除了优先级之外，Android 还使用 Linux cgroups 限制 CPU 资源。背景优先级的线程会被移动到 bg_non_interactive cgroup，如果其他组中的线程繁忙，该 cgroup 仅限于可用 CPU 的 5%。

配置 HandlerThread 时将 THREAD_PRIORITY_MORE_FAVORABLE 添加到 THREAD_PRIORITY_BACKGROUND 会将线程移动到默认 cgroup，但请始终考虑是否真的有必要——通常没有必要！

下表详细说明了 Android 线程优先级到 Linux nice 级别的映射；但是，不建议在常规应用程序中使用低于 -2 的 nice 级别：

| Java 优先级 | 线程优先级 | Nice 级别 |
|---|---|---|
| | THREAD_PRIORITY_URGENT_AUDIO | -19 |
| | THREAD_PRIORITY_AUDIO | -16 |
| MAX_PRIORITY | THREAD_PRIORITY_URGENT_DISPLAY | -8 |
| | THREAD_PRIORITY_DISPLAY | -4 |

