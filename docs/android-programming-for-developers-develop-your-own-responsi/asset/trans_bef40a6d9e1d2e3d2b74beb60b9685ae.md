

```java
task = new DownloadImageTask();
url = new URL(getArguments().getString("url"));
task.execute(url);
}
// Called when an activity is attached
public void onAttach(Activity activity) {
super.onAttach(activity);
listener = (AsyncListener)activity;
}
```

```java
public void onDetach() {
super.onDetach();
listener = null;
}
// Cancel the download
public void cancel() {
if (task != null) {
task.cancel(false);
}
}
```

```java
private class DownloadImageTask extends AsyncTask<URL, Integer,
Bitmap> {
// ... doInBackground elided for brevity ...               }
}
```

正如您所知道，Fragment 的生命周期与其所属的 Activity 绑定，因此回调会按照当前 Activity 的生命周期事件有序调用。例如，当 Activity 停止时，所有附加到它的 Fragment 都会被分离，并收到 Activity 状态变更的通知。

在我们的示例中，我们使用 Fragment 的生命周期方法（onAttach 和 onDetach）来保存或移除保留 Fragment 中的当前 Activity 引用。

当 Activity 附加到我们的 Fragment 时，会调用 onCreate 方法来创建私有的 DownloadImageTask 对象，随后调用 execute 方法在后台启动下载。

newInstance 静态方法用于初始化和设置新 Fragment，无需调用其构造函数和 URL Setter。一旦创建 Fragment 对象实例，我们就会使用 setArguments 函数将图片 URL 保存在 Fragment arguments 成员存储的 Bundle 对象中。如果 Android 系统恢复我们的 Fragment，它会调用无参数的默认构造函数，并且还可以使用旧的 Bundle 来重建 Fragment。

每当在配置更改期间 Activity 被销毁并重新创建时，setRetainInstance(true) 会强制 Fragment 在 Activity 回收过渡期间存活。正如您所察觉，当 Activity 因配置更改被销毁时，此技术在我们不希望重新创建开销大的对象或具有独立生命周期的对象的情况下可能非常有用。

## 注意

需要知道的是，retainInstance() 只能用于不在返回栈中的 Fragment。对于保留的 Fragment，当 Activity 重新附加到新 Activity 时，不会调用 onCreate() 和 onDestroy()。

接下来，我们的 Fragment 需要管理并执行 DownloadImageTask，该任务通过 AsyncListener 接口将进度更新和结果代理回 Activity：

```java
private class DownloadImageTask extends AsyncTask<URL, Integer,
Bitmap> {
...
protected void onPreExecute() {
if (listener != null)
listener.onPreExecute();
}
protected void onProgressUpdate(Integer... values) {
if (listener != null)
listener.onProgressUpdate(values);
}
protected void onPostExecute(Bitmap result) {
if (listener != null)
listener.onPostExecute(result);
}
protected void onCancelled(Bitmap result) {
if (listener != null)
listener.onCancelled(result);
}
}
```

如前所述，AsyncListener 是负责使用后台任务返回的结果更新 UI 的实体。

现在，我们需要的是一个实现 AsyncListener 并使用 DownloadImageHeadlessFragment 来执行长时间运行任务的宿主 Activity。完整源代码可从 Packt Publishing 网站下载，因此我们只需查看重点部分：

```java
public class ShowMyPuppyHeadlessActivity
    extends FragmentActivity implements
    DownloadImageHeadlessFragment.AsyncListener {

private static final String DOWNLOAD_PHOTO_FRAG =
        "download_photo_as_fragment";

...

@Override
protected void onCreate(Bundle savedInstanceState) {
    ...
    FragmentManager fm = getSupportFragmentManager();
    downloadFragment = (DownloadImageHeadlessFragment)
    fm.findFragmentByTag(DOWNLOAD_PHOTO_FRAG);

    // If the Fragment is non-null, then it is currently being
    // retained across a configuration change.
    if (downloadFragment == null) {
    downloadFragment = DownloadImageHeadlessFragment.
        newInstance("http://img.allw.mn/content" +
            "www2009/03/april1.jpg");
    fm.beginTransaction().add(downloadFragment,
        DOWNLOAD_PHOTO_FRAG).
        commit();
    }
}
```

首先，在 onCreate 回调中创建 Activity 时，我们会检查 Fragment 是否已存在于 FragmentManager 中，只有在缺失时才创建实例。

创建 Fragment 时，我们通过 newInstance 方法构建 Fragment 实例，然后将其推送到 FragmentManager，这是负责存储并执行切换的实体。

如果我们的 Activity 已重启，在接收到进度更新回调时需要重新显示进度对话框，因此我们会在更新进度条之前检查并在必要时显示它：

```java
@Override
public void onProgressUpdate(Integer... value) {
    if (progress == null)
        prepareProgressDialog();

    progress.setProgress(value[0]);
}
```

最后，Activity 需要实现 AsyncListener 定义的 onPostExecute 和 onCancelled 回调。onPostExecute 将如之前示例一样更新 resultView，两者都会执行一些清理工作——关闭对话框并移除 Fragment，因为其工作现已完成：

```java
@Override
public void onPostExecute(Bitmap result) {
    if (result != null) {
        ImageView iv = (ImageView) findViewById(
                R.id.downloadedImage);
        iv.setImageBitmap(result);
    }
    cleanUp();
}

// When the task is cancelled the dialog is dismissed
@Override
public void onCancelled(Bitmap result) {
    cleanUp();
}
```

```java
// Dismiss the progress dialog and remove the
// the fragment from the fragment manager
private void cleanUp() {
    if (progress != null) {
        progress.dismiss();
        progress = null;
    }

    FragmentManager fm = getSupportFragmentManager();
    Fragment frag = fm.findFragmentByTag(DOWNLOAD_PHOTO_FRAG);
    fm.beginTransaction().remove(frag).commit();
}
```

这种技术在 Android 开发社区中广为人知，称为无头 Fragment，它简单且一致，因为每次发生配置更改时，它都会将重新创建的 Activity 附加到无头 Fragment 上。在 Fragment 上维护一个 Activity 引用，并在 Fragment 附加（Activity 创建）和分离（Activity 销毁）时进行更新。

利用此模式，AsyncTask 无需关注配置更改不可预测的发生情况，也无需担心工作完成后的 UI 更新，因为它会将生命周期回调转发给当前 Activity。

## AsyncTask 的应用

现在我们已经了解了如何使用 AsyncTask，可能会问自己什么时候应该使用它。

AsyncTask 的良好候选应用通常是寿命较短的操作（最多一两秒），这些操作直接关联到特定的 Fragment 或 Activity，并且需要更新其用户界面。

AsyncTask 非常适合运行短暂的 CPU 密集型任务，例如数值计算或在大文本字符串中搜索单词，将其从主线程移开，以便主线程能够保持对输入的响应并维持高帧率。

阻塞式 I/O 操作（例如读写文本文件，或使用 BitmapFactory 从本地文件加载图像）也是 AsyncTask 的良好用例。

当然，也有 AsyncTask 不太适合的用例。对于任何需要超过一两秒的操作，我们应该权衡如果用户旋转设备、在应用或 Activity 之间切换，或发生其他我们无法控制的情况时，重复执行此操作的成本。

考虑到这些因素，以及当我们尝试处理它们时复杂性增加的速度（例如，保留的无头 Fragment！），AsyncTask 对于较长操作开始失去光彩。

AsyncTask 经常用于从远程 Web 服务器获取数据，但这可能会遇到我们之前研究过的 Activity 生命周期问题。最终用户可能正在使用不稳定的 3G 或 HSDPA 连接，网络延迟和带宽可能差异很大，完整的 HTTP 请求-响应周期很容易持续数秒。这在上传大量数据（例如图像）时尤为重要，因为可用带宽通常是非对称的。

虽然我们必须在主线程之外执行网络 I/O，但 AsyncTask 不一定是理想选择——如我们稍后所见，有更合适的构造可用于将此类工作从主线程卸载。

