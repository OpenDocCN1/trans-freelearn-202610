

## 处理异常

AsyncTask 定义的回调方法规定我们不能抛出受检异常，因此必须使用 try/catch 块包装任何抛出受检异常的代码。从 AsyncTask 的方法中传播出的未检查异常会导致我们的应用程序崩溃，因此我们必须仔细测试并在必要时处理这些异常。

对于在主线程上运行的回调方法——onPreExecute()、onProgressUpdate()、onPostExecute() 和 onCancelled()——我们可以在方法中捕获异常，并直接更新用户界面以提醒用户。

当然，异常也可能在我们的 doInBackground() 方法中发生，因为这是 AsyncTask 完成大部分工作的地方，但不幸的是，我们无法从 doInBackground() 更新用户界面。一个简单的解决方案是让 doInBackground() 返回一个可能包含结果或异常的对象。首先，我们将创建一个泛型类来存储操作的结果，以及一个用于存储异常的成员：

```
public class Result<T> {
	public T result;
	public Throwable error;
}
```

下一步，我们将创建一个名为 SafeDownloadImageTask 的新下载 AsyncTask，它负责异常处理，并且结果类型为 Result<Bitmap> 而不是 Bitmap：

```
public class SafeDownloadImageTask extends AsyncTask<URL, Integer, Result<Bitmap>> {
	// Method executed on the Background Thread
	protected Result<Bitmap> doInBackground(URL... params) {
		Result<Bitmap> result = new Result<Bitmap>();
		try {
			// elided for brevity ...
			...
			result.result = bitmap;
		} catch (Throwable e) {
			result.error = e;
		} ...
		return result;
	} ...
}
```

现在，我们可以在 onPostExecute 中检查 Result 对象中是否存在异常。如果存在，我们可以处理它，例如提醒用户；否则，我们像往常一样使用实际结果，并从结果中使用位图：

```
@Override
protected final void onPostExecute(Result<Bitmap> result) {
    ...
    if ( result.error!= null ) {
    // ... alert the user ...
    ...
    Log.e("SafeDownloadImageTask",
    "Failed to download image ",result.exception);
    loadDefaultImage(imageView);
    } else {
    // ... success, continue as normal ...
    imageView.setImageBitmap(result.actual);
    }
}
```

像上面这样的安全实现可以确保，在后台线程抛出的任何错误都会被安全地转发到主线程，而不会影响 AsyncTask 的正常生命周期。让我们尝试检索一个不存在的图片，看看异常是否被正确处理：

```
URL url = new URL("http://img.allw.mn" +
    "contentwww/2009/03/notfound.jpg");
new SafeDownloadImageTask(ShowMyPuppyActivity.this, iv)
.execute(url);
```

正如预期的那样，错误被捕获，包装在 Result 对象中，并在 Android 日志中打印，堆栈跟踪指向 SafeDownloadImageTask.doInBackground 方法：

```
...downloadBitmap(SafeDownloadImageTask.java:85)
...doInBackground(SafeDownloadImageTask.java:60)
...
```

```
84: if (responseCode != HttpURLConnection.HTTP_OK){
85:         throw new Exception(...);
86: }
```

## 控制并发级别

到目前为止，我们一直小心避免过于具体地说明调用 AsyncTask execute 方法时到底会发生什么。我们知道 doInBackground() 将在主线程之外执行，但这究竟意味着什么？

AsyncTask 的最初目标是帮助开发者避免阻塞主线程。在 API 级别 3 的初始形式中，AsyncTask 在单个后台线程上被排队并串行执行（即一个接一个），保证它们将按照启动顺序完成。

在 API 级别 4，这发生了变化，改为使用最多 128 个线程的池来并发执行多个 AsyncTask——并发级别高达 128。乍一看，这似乎是一件好事，因为 AsyncTask 的常见用例是执行阻塞 I/O，其中线程大部分时间都在空闲等待数据。

然而，正如我们在第 1 章“Android 中的异步编程”中所见，并发编程中经常出现许多问题，事实上，Android 团队意识到，默认情况下并发执行 AsyncTask 会使开发者面临潜在的编程问题（例如，并发执行时，不能保证 AsyncTask 将以启动的相同顺序完成）。

因此，在 API 级别 11 又做了一项进一步的更改，切换回默认串行执行，并引入了一种将并发控制权交还给应用开发者的新方法：

```
public final AsyncTask<Params, Progress, Result>
executeOnExecutor(Executor exec, Params... params)
```

从 API 级别 11 开始，我们可以使用 executeOnExecutor 启动 AsyncTask，并通过提供一个 Executor 对象来自行选择并发级别。

Executor 是 JDK java.util.concurrent 包中的一个接口，如在第 1 章“Android 中的异步编程”中更详细地描述的那样。

它的目的是提供一种提交任务以供执行的方式，而无需明确说明执行将如何或何时进行。Executor 的实现可能使用单个线程顺序运行任务，使用有限的线程池来控制并发级别，甚至为每个任务直接创建一个新线程。

AsyncTask 类提供了两个 Executor 实例，允许您在本节前面描述的并发级别之间进行选择：

- SERIAL_EXECUTOR：此 Executor 将任务排队，并确保任务由 AsyncTask 线程池按提交顺序顺序执行。
- THREAD_POOL_EXECUTOR：此 Executor 使用线程池运行任务以提高效率（启动新线程会带来一些开销成本，可以通过池化和复用来避免）。THREAD_POOL_EXECUTOR 是 JDK 类 ThreadPoolExecutor 的一个实例，它使用一个根据需求增长和收缩的线程池。在 AsyncTask 的情况下，该池配置为至少维护五个线程，并扩展到最多 128 个线程。在 Android Lollipop 5.0（API 级别 21）中，线程的最大数量减少到 CPU 核心数 * 2 + 1，ThreadPool 全局排队容量增加。

要使用特定的执行器执行 AsyncTask，我们调用 executeOnExecutor 方法，提供我们要使用的执行器的引用，例如：

```
task.executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR,
 params);
```

由于自 API 级别 11 起，execute 的默认行为是在单个后台线程上串行运行 AsyncTask，因此以下两条语句是等价的：

```
task.execute(params);
 task.executeOnExecutor(AsyncTask.SERIAL_EXECUTOR, params);
```

在下图中，我们将展示当串行执行器和线程池处理一组按顺序入队的 AsyncTask 时的区别：

```
new SleepAsyncTask(1).execute(1000);
```

```
new SleepAsyncTask(4).execute(1000);
```

![](img/35d01831c19688a44e50bce7c489007a_1034_0.png)

如上图所示，串行执行器使用 AsyncTask 线程池中可用的线程，但它们只会在前一个 AsyncTask 完成后才处理下一个 AsyncTask。或者，ThreadPoolExecutor 会在有可用线程完成工作时立即开始处理下一个任务，而不保证它们将以启动顺序完成：

### 注意

需要注意的是，系统中所有的 AsyncTask 都将共享同一个静态执行器 AsyncTask.THREAD_POOL_EXECUTOR。对于 SerialExecutor 来说，情况更糟，因为如果一个 AsyncTask 长时间占用单个执行器，接下来的任务将在队列中等待处理。

除了 AsyncTask 提供的默认执行器和 java.util.concurrent 中可用的执行器之外，我们还可以选择创建自己的执行器。例如，我们可能希望通过操作一个小

