

```java
public static final String SYNC_FILE_KEY = "file";
public static final String SYNC_ENDPOINT_KEY = "http_endpoint";

// Variable used to store a reference to the service
private final JobService jobService;

// Constructor
public SyncTask(JobService jobService) {
    this.jobService = jobService;
}

...
```

作为起点，我们指定了泛型 AsyncTask 类的参数类型，将 JobParameters 设置为 doInBackground 的参数，将 Result 设置为从 doInBackground 返回并传递给 onPostExecute 函数的类型。

稍后，我们创建用于在 bundle 中传递信息的最终常量键。

Result 类型也会从之前的会话中恢复，以便在后台执行期间出现问题时返回错误。

不深入细节，我们将实现负责将数据上传到远程 Web 服务的 doInBackground 代码：

```java
@Override
protected Result<JobParameters> doInBackground(
        JobParameters... params) {

    Result<JobParameters> result = new Result<JobParameters>();
    HttpURLConnection urlConn = null;
    try {
        URL url;
        ...
        // Retrieve the file to upload from the parameters
        // passed in
        String file = params[0].getExtras().
                getString(SYNC_FILE_KEY);

        // Remote WebService Path
        String endpoint = params[0].getExtras().
                getString(SYNC_ENDPOINT_KEY);

        url = new URL("http://<webs_host>:<webs_port>/"
                + endpoint);

        ...
        // Load the account information stored internally
        String body = Util.loadJSONFromFile(jobService.file);
```

```java
...ing body, urlConn.getOutputStream().write(body);
// Write the information to the remote service
uploadJsonToServer(urlConn, body);
// Process Server Response
...
} catch (Exception e) {
    result.exc = e;
} finally {
    if (urlConn != null) {
        urlConn.disconnect();
    }
}
return result;
}
```

这里为简洁起见省略了一些实现细节，但我们已实现 doInBackground 函数以读取设备文件中内部存储的 JSON 数据，并通过 HttpURLConnection 上传它。显示表单的 Android Activity 会将保存和同步按钮提供给最终用户。**保存** 按钮一旦被按下，在点击 **保存** 按钮时会将账户信息存储在 account.json 本地文件中。

点击 **同步** 按钮后，将安排作业以与我们的远程 HTTP 服务器同步数据。当我们定义的作业条件满足时，将调用 doInBackground 在后台执行同步过程。

既然我们已拥有将数据上传到服务器的代码，让我们通过处理响应和服务器错误来完成它：

```java
try {
    ...
    int resultCode = urlConn.getResponseCode();
    if (resultCode != HttpURLConnection.HTTP_OK) {
        throw new Exception("Failed to sync with server :" + resultCode);
    }
    result.result = params[0];
    ...
}
```

当发生异常时，例如服务器宕机或服务器内部错误，异常会通过我们的 Result 对象传播到 onPostExecute 以供进一步处理。

请注意，我们正在小心处理错误情况，因此，为了向用户通知后台工作的结果，我们将编写一个在主线程上运行的 onPostExecute 函数，它将发布系统通知，告知用户任务是成功完成还是惨遭失败：

```java
@Override
protected void onPostExecute(Result<JobParameters> result) {
    NotificationCompat.Builder builder =
        new NotificationCompat.Builder(jobService);

    ...

    if (result.exc != null) {
        // Error handling
        jobService.jobFinished(result.result, true);
        builder.setContentTitle("Failed to sync account")
            .setContentText("Failed to sync account " + result.exc);
    } else {
        // Success handling
        builder.setContentTitle("Account Updated")
            .setContentText("Updated Account Successfully at " +
                new Date().toString());
        jobService.jobFinished(result.result, false);
    }
    nm.notify(NOTIFICATION_ID, builder.build());
}
```

任务完成后，我们调用 jobFinished(JobParameters params, boolean needsRescheduled) 以告知系统该任务已完成；但是，当发生异常时，我们会向系统报告任务未能成功完成，即在 jobFinished 的第二个参数中传递 true。

当已完成的作业失败并需要重新调度时，我们将 false 作为 jobService.jobFinished 的第二个参数传递，Scheduler API 将使用 JobInfo 对象中指定的回退时间重新调度我们的作业。但是，由于我们的作业仅在空闲模式下执行，失败的作业将被添加到调度程序队列中，并在未来的空闲维护窗口内重新执行，而不使用 JobInfo 中指定的回退时间。

始终调用 jobFinished 非常重要，以释放分配给作业的 WakeLock，并通知系统可以处理更多作业。

如果一切顺利，通知抽屉中应该会出现一条通知，显示成功消息以及上次同步成功的时间。

最后，我们可以更新 SyncJobService 代码以启动和停止 SyncTask 执行：

```java
public class SyncJobService extends JobService {
    private static final String TAG = "SyncJobService";
    SyncTask mJob = null;
    @Override
    public boolean onStartJob(JobParameters params) {
        Log.i(TAG, "on start job: " + params.getJobId());
        if (mJob != null) {
            mJob = new SyncTask(this);
            mJob.execute(params);
            return true;
        }
        return false;
    }

    @Override
    public boolean onStopJob(JobParameters params) {
        Log.i(TAG, "on stop job: " + params.getJobId());
        if (mJob != null) {
            mJob.cancel(true);
            mJob = null;
        }
        return true;
    }
}
```

## 列出待处理的作业

与 AlarmManager API 不同，Scheduler API 提供了列出应用程序所有待处理计划的功能。这个实用功能可以帮助我们识别将来要执行的作业，并根据该列表做出相应反应。检索到的列表可以帮助我们定位想要取消的待处理作业。

JobScheduler 服务类具有一个具有以下签名的实例方法：

```java
public List<JobInfo> getAllPendingJobs();
```

该方法将返回一个 JobInfo 对象列表，我们可以使用它在作业构建期间观察作业参数集：

- 每个作业的作业条件：
  - getNetworkType()
  - isRequireDeviceIdle()
  - isRequireCharging()
  - getMinLatencyMillis()
  - isPeriodic()
  - getIntervalMillis()
  - isPersisted()
  - getMaxExecutionDelayMillis()
- 由 JobScheduler 回调以执行作业的 JobService 子类—getService()
- 作业参数：getExtras()
- 重试策略：getInitialBackoffMillis() 和 getBackoffPolicy()

好了，现在我们准备创建一个 Activity 来打印应用程序的待处理作业列表：

```java
public class JobListActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        JobScheduler jobScheduler = (JobScheduler)
            getSystemService(JOB_SCHEDULER_SERVICE);

        // Get the list of scheduled jobs
        List<JobInfo> jobList = jobScheduler.getAllPendingJobs();
        // Initialize the adapter job list
        JobListRecyclerAdapter adapter =
            new JobListRecyclerAdapter(this, jobList);

        rv.setAdapter(adapter);
        // Set the Job Counter
        TextView jobCountTv = (TextView)findViewById(R.id.jobCount);
        jobCountTv.setText(Integer.toString(jobList.size()));
    }
}
```

为了在 UI 中列出待处理的作业，我们使用了支持库的 RecyclerView 类，它是 ListView 的更高级版本，简化了大量 View 的创建。

首先，我们将构建 ViewHolder 以持有行视图的引用，这些行视图将显示 jobId 和服务端点：

```java
public class JobListRecyclerAdapter extends
    RecyclerView.Adapter<JobListRecyclerAdapter.JobViewHolder> {
    ...

    public static class JobViewHolder extends
    RecyclerView.ViewHolder {
    // References to the Views
    CardView cv;
    TextView jobId;
    TextView serviceName;

    JobViewHolder(View itemView) {
        super(itemView);
        cv = (CardView)itemView.findViewById(R.id.cv);
        jobId = (TextView)itemView.findViewById(R.id.JobIdTv);
        serviceName = (TextView)
            itemView.findViewById(R.id.className);
    }
}
```

为了将 jobInfo 参数绑定到当前 ViewHolder，我们将编写 RecyclerView.onBindViewHolder 以根据当前 JobInfo 设置信息：

```java
@Override
```

