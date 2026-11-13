
backoff 初始值，用于确定重试之间的间隔和重试增加策略。JobScheduler API 提供两种策略，用于定义后续尝试之间重试时间的增加方式：

- BACKOFF_POLICY_LINEAR: 重试之间的间隔时间线性增加—initial_backoff_millis * num_retries
- BACKOFF_POLICY_EXPONENTIAL: 重试之间的间隔时间呈指数增长—initial_backoff_millis * 2 ^ (num_retries)

![](img/35d01831c19688a44e50bce7c489007a_1056_0.png)

退避间隔将持续增加，直到达到五小时的退避时间（JobInfo.MAX_BACKOFF_DELAY_MILLIS），初始默认值为 30 秒（JobInfo.DEFAULT_INITIAL_BACKOFF_MILLIS）。

现在，在以下示例中，我们将展示如何为 jobInfo 对象创建线性退避策略和指数退避策略：

```
jobBuilder.setBackoffCriteria(TimeUnit.MINUTES.toMillis(10L),
JobInfo.BACKOFF_POLICY_LINEAR);
```

```
jobBuilder.setBackoffCriteria(TimeUnit.MINUTES.toMillis(3),
JobInfo.BACKOFF_POLICY_EXPONENTIAL);
```

构建器类还提供了一种方法来设置一些参数，以通过 PersistableBundle 对象转发作业：

```
PersistableBundle bundle = new PersistableBundle();
bundle.putInt(MY_JOB_ARG1, 2);
jobBuilder.setExtras(bundle);
```

## 注意

PersistableBundle 是一种特殊的 bundle，可以稍后保存和恢复。其主要目的是将参数传递给延迟作业执行。

一旦我们拥有调度所定义作业的所有条件，就可以构建 JobInfo 并使用它将作业执行纳入我们的应用程序中：

```
JobInfo.Builder jobBuilder = ...
// Set Criterias
JobInfo jobInfo = jobBuilder.setRequiresCharging(true)
        .setRequiresDeviceIdle(true)
        ...
        .build();
```

## 调度作业

在已定义条件和 JobInfo 对象的基础上，我们拥有设置应用程序作业所需的所有实体。那么现在让我们展示如何通过真实示例创建作业。

我们的示例将使用 Scheduler API 调度的作业，同步存储在设备文件中的用户账户信息与 HTTP Web 服务。用户界面将提供一个 UI，用于更新用户信息，一个按钮用于将信息保存到内部文件，以及一个按钮用于设置同步作业，该作业将把账户信息上传到 Web 服务。

首先，让我们定义作业的先决条件和参数：

- 我们的作业仅在设备充电时运行，以节省电池电量
- 我们的作业仅在有非按流量计费网络可用时运行，以节省移动网络带宽
- 我们的作业仅在设备空闲时运行，因为我们不想降低 UI 响应速度
- 我们的作业必须在调度后的八小时内至少运行一次
- 我们的作业应在设备重启后继续运行

JobInfo 对象需要一个 ID 来在所有 JobSchedule 方法中标识作业，因此为了确保一致性，使用公共静态 int 来标识它是个好主意：

```
static final int ACC_BACKUP_JOB_ID = "AccountBackJobService".hashCode();
```

后续取消或列出创建的作业的调用必须使用此处定义的相同 jobId。

由于我们使用文件在设备内部存储账户信息，因此用于检索账户信息的文件名需要作为参数传递给作业。同样的原则也适用于远程 Web 服务端点。

为了转发所需参数，我们需要构建 PersistableBundle，将文件名和端点路径作为 bundle 参数传递：

```
private static final String SYNC_FILE = "account.json";
private static final String SYNC_PATH = "account_sync";
private static final String SYNC_PATH_KEY = "path";
```

```
PersistableBundle bundle = new PersistableBundle();
// Forward filename where the account information is stored
bundle.putString(SyncTask.SYNC_FILE_KEY, SYNC_FILE);
// Forward the HTTP Path used to upload the account information
bundle.putString(SyncTask.SYNC_PATH_KEY, SYNC_PATH);
```

一旦条件确定并且我们拥有服务的标识符和类名，就可以使用 Builder 内部类创建 JobInfo，如下面的代码所示：

```
ComponentName serviceName = new ComponentName(this,
        AccountBackupJobService.class);
```

```
JobInfo jobInfo = new JobInfo.Builder(ACC_BACKUP_JOB_ID, serviceName)
        .setRequiresCharging(true)
        .setRequiredNetworkType(JobInfo.NETWORK_TYPE_UNMETERED)
        .setRequiresDeviceIdle(true)
        .setPersisted(true)
        .setOverrideDeadline(TimeUnit.HOURS.toMillis(8L))
        .setExtras(bundle)
        .build();
```

现在我们准备使用 JobScheduler 服务调度作业：

```
JobScheduler jobScheduler = (JobScheduler)
        getSystemService(JOB_SCHEDULER_SERVICE);
int result = jobScheduler.schedule(jobInfo);
```

```
if (result == JobScheduler.RESULT_FAILURE) {
    // Failed to setup the job
    Toast.makeText(AccountInfoActivity.this,
            "Failed to setup a sharedpref backup job",
            Toast.LENGTH_SHORT).show();
} else {
    // Schedule Success
    Toast.makeText(SharedPrefActivity.this,
            "SharedPrefBack job successfully scheduled",
            Toast.LENGTH_SHORT).show();
}
```

JobScheduler 的 schedule 方法在失败时会返回 RESULT_FAILURE，在成功时会返回我们在 JobInfo.Builder 构造函数中定义的作业标识符。

现在，既然我们刚刚调度了作业，是时候在 JobService 子类中编写备份行为了。在下一节中，我们将详细介绍如何实现一个与 JobScheduler 框架协同工作的 JobService。

## 实现 JobService

我们的 JobService 子类是负责执行繁重工作的实体，一旦 JobInfo 中指定的所有条件都满足，它就会接收回调。要实现自己的服务，我们需要继承 JobService 并覆盖启动和停止回调，如下面的代码所示：

```
public class AccountBackupJobService extends JobService {
    @Override
    public boolean onStartJob(JobParameters params) {
        // Start your job here
        return false;
    }

    @Override
    public boolean onStopJob(JobParameters params) {
        // Stop your job here
        return false;
    }
}
```

onStartJob 回调函数在主线程上运行，如果作业需要执行异步处理，则应返回 true 以表示它仍在后台线程上执行工作。回调函数还接收 JobInfo bundle 中指定的额外参数。

当系统需要取消作业执行时（因为 jobInfo 中指定的条件不再满足），onStopJob 会被系统自动调用。

例如，我们的作业要求在设备处于空闲状态时执行工作，因此，如果用户开始与设备交互导致设备退出空闲模式，onStopJob 会被调用以暂时放弃执行。

在此函数中，我们应该释放分配给作业的任何资源并停止任何正在进行的后台处理。此函数返回的布尔值将指示你是否希望根据作业创建时指定的相同条件重试此作业，或放弃作业执行。你应该使用 true 根据作业创建期间指定的重试条件重新调度此作业。

在添加服务业务逻辑之前，我们必须将服务类添加到 AndroidManifest.xml，并使用 android.permission.BIND_JOB_SERVICE 权限保护我们的服务：

```xml
<service
    android:name=".chapter7.AccountBackupJobService"
    android:exported="true"
    android:permission="android.permission.BIND_JOB_SERVICE" />
```

关于你的 JobService 实现，有两点需要牢记：

- 首先，onStartJob 和 onStopJob 回调将在主线程上运行，你有责任将服务的长时间运行任务交给独立线程，以防止主线程上的阻塞操作导致应用程序出现 ANR 对话框。
- 其次，Android 系统会在你的 JobService 回调运行期间为你获取并持有 WakeLock，或者直到你在 onStartJob 函数返回 true 的情况下显式调用 jobFinished 方法。如果你不这样做，系统就会认为你的作业执行尚未完成。WakeLock 会使设备保持唤醒状态并白白浪费设备电池电量。这可能会让用户生气，并成为卸载应用程序的理由，因为你的应用程序会浪费资源和电池并影响用户体验。

现在我们已经了解了 JobService 的理论知识，让我们编写代码，在后台处理线程中远离主线程执行与远程服务器的账户同步。

考虑到迄今为止学到的 Android 构造，我们将使用在第 3 章“探索 AsyncTask”中学到的 AsyncTask 构造，因其简单性，并创建一个 AsyncTask 子类，使用为此目的创建的内容上传账户信息：

```
public class Result<T> {
    public T result;
    public Exception exc;
}
```

```
public class SyncTask extends
    AsyncTask<JobParameters, Void, Result<JobParameters>> {

    // Parameter Keys for parameter arguments
    public static final String SYNC_FILE_KEY = "file";
```
