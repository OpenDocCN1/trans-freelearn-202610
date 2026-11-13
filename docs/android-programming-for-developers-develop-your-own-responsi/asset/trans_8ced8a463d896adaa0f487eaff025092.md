

@Override
public void onBindViewHolder(
 JobListRecyclerAdapter.JobViewHolder holder, int position) {
 // Retrieve the job for the current list row
 final JobInfo ji = mJobList.get(position);
 // Update the UI Views with the Job Info
 holder.jobId.setText(Integer.toString(ji.getId()));
 holder.serviceName.setText(ji.getService().getClassName());
}

是的，多亏了 getAllPendingJobs，我们拥有了一份作业列表，而且，我们可以对其进行程序化分析，以围绕当前的应用程序情况创建行为。

某些代码被故意省略；但是，完整源代码可在 Packt Publishing 网站上查看。请查看完整源代码，以了解 Recycler View 和 Card View 如何用于构建作业列表界面。

要随心所欲地操作作业，本章只需涵盖一项 CRUD（创建、读取、更新、删除）操作，即删除操作。删除作业操作由取消函数提供，并将在下一节中详细介绍。

## 取消作业

在某些情况下，当环境情况发生变化或不再有必要执行作业时，我们希望为用户提供取消作业的能力——例如，用户更改了作业所依赖的某项信息，该作业不再适用。JobScheduler 服务通过以下 cancel 和 cancelAll 方法为作业取消提供支持：

```
void cancel(int jobId);

void cancelAll();
```

第一个方法 cancel(jobId) 允许我们使用 schedule(JobInfo job) 函数返回的作业标识符，或 getAllPendingJobs 函数返回的 JobInfo 对象上可用的 jobId，来取消特定作业。

cancelAll() 方法允许我们取消当前应用已在 JobScheduler 中注册的计划作业。

借助前面示例中的 JobInfo，我们可以通过传递作业标识符来取消特定作业：

```
final JobInfo ji = ...;
JobScheduler jobScheduler = (JobScheduler)
mContext.getSystemService(mContext.JOB_SCHEDULER_SERVICE);
// Cancel a Specific Job based on the JobInfo->jobId
jobScheduler.cancel(ji.getId());
```

每当我们取消一个计划时，该作业将从 JobScheduler 的未来执行队列中移除，不再由 SyncJobService 或任何其他 JobService 执行。

### 调度周期性作业

到目前为止，我们已调度了一次性作业，但您知道是否有一个选项可以按固定间隔调度作业执行吗？这类作业非常适合用于执行重复备份或重复网络操作，例如应用程序用户数据备份。

让我们更新 AccountInfoActivity 以调度账户同步作业的周期性执行。

我们将首先为周期性作业定义一个新的作业标识符：

```
static final int SYNC_PER_JOB_ID = "SyncJobPerService".hashCode();
```

我们可以这样调度一个大约每 12 小时执行一次的周期性作业：

```
JobInfo.Builder builder = new JobInfo.Builder(SYNC_PER_JOB_ID,
serviceName);
builder.setRequiresDeviceIdle(true)
// Persist the schedule across the device reboots
.setPersisted(true)
.setPeriodic(TimeUnit.HOURS.toMillis(12L))
.setRequiredNetworkType(JobInfo.NETWORK_TYPE_UNMETERED)
.setRequiresDeviceIdle(true)
.setExtras(bundle);
```

```
// Schedule the repeating job
JobScheduler jobScheduler = (JobScheduler)
getSystemService(JOB_SCHEDULER_SERVICE);
jobScheduler.schedule(builder.build());
```

现在，我们可以在设备空闲且 Wi-Fi 网络可用时，在后台周期性运行同步作业。作业计划将由系统保留，并在设备启动后重新启用，直到我们显式取消作业或通过 cancelAll() 取消所有作业。

### JobScheduler 的应用

JobScheduler API 允许我们在某些条件下调度未来在无需用户干预的情况下异步运行的工作。该 API 还可以通过将作业执行推迟到设备充电或连接到无流量限制的网络（例如 Wi-Fi 网络）时，来降低能耗。

理想场景包括可推迟且无需精确定时执行的应用程序数据库备份、向网络定期上传用户数据以及下载配置参数。也就是说，通常是不必立即运行且数据无需立即供用户使用的作业。在不影响用户体验的前提下降低应用程序能耗，将延长设备电池续航时间，从而提升用户体验。

JobScheduler 可以覆盖 AlarmManager 的大多数用例，尽管它提供了优化设备资源获取的高级功能。作为一项额外功能，此 API 提供了一种创建可在设备关机和重启后仍然存在的计划的方法。

唯一的重大缺点是 JobScheduler 仅在 Android 5.0（Lollipop）中引入。因此，您需要将应用程序的目标 API 版本设置为 21 或更高版本，才能与此 API 交互。

撰写本文时（2016 年 7 月），45% 的 Android 设备运行支持 JobScheduler 的 Android 版本。如需获取按版本划分的 Android 市场份额的最新信息，请查看 Android 开发者信息中心。

## 摘要

在本章中，我们探索了 `JobScheduler` API，并将其用于调度在我们定义的一组条件满足时启动的后台工作。

我们详细了解了如何设置 API 支持的不同条件，以及如何基于 `JobInfo` 对象进行调度，该对象在设备充电且未在使用时启动作业。

同时，我们学习了如何实现异步 `JobService`，它能够在后台执行线程中运行，并通过释放所有已获取的资源（`WakeLock` 等）来正确完成作业执行。

后来，我们使用 `getAllPendingJobs` 创建示例代码，列出应用程序中所有待处理的 Scheduler API 作业。从示例中，我们学习了如何取消应用程序中的特定作业或所有待处理作业。

最后，我们使用 `JobScheduler` API 构建了一个持久且重复的计划，该计划每 12 小时唤醒设备并执行我们的作业。

在下一章中，我们将学习如何使用高效的异步库和协议进行网络数据传输，而无需过度消耗电池电量。

## 第 5 章. 与网络交互

到目前为止，我们一直使用 HttpURLConnection HTTP 客户端来传输来自网络或发送到网络的数据，例如从 HTTP 服务器下载图像以及与远程 HTTP 服务器同步信息。我们一直在盲目地使用这个 Android HTTP 客户端，而没有深入了解其内部机制以及这个方便的框架为我们透明处理 HTTP 协议所提供的功能。

在本章中，我们将了解 HttpURLConnection 的高级功能，以及使用 HTTP 协议与远程服务器进行异步和安全通信的新技术。

同时，我们将学习如何使用自定义 HTTP 客户端通过安全通道进行通信、调整 HTTP 客户端以应对网络延迟，以及学习如何与 Web API 交互。

在本章中，我们将涵盖以下内容：

- 介绍 Android HTTP 客户端
- 异步执行 HTTP 请求
- 与 JSON Web API 交互
- 与 XML Web API 交互
- 优化 HTTP 超时
- 通过 SSL 会话安全通信
- HTTP 开源库

### 介绍 Android HTTP 客户端

近来，从远程服务器发送和接收数据的能力已成为所有应用程序都应具备的基本功能，以创建动态且令人印象深刻的体验。如今，几乎每个应用程序都使用网络来获取数据信息、执行远程业务逻辑操作以及下载或上传资源。

应用程序与远程服务器之间发生的网络交互通常定义为一组使用网络协议穿越网络的请求/响应消息。

一般来说，HTTP 协议常用于在对等体之间传输消息，Android SDK 随附两个可直接用于发送和接收数据的高级 HTTP 客户端：AndroidHttpClient 和 HttpURLConnection。

HTTP 通信协议是一种无状态的标准文本应用层协议，由互联网工程任务组（IETF）和万维网联盟（W3C）维护，并在互联网上广泛用于客户端（通常称为用户代理）与服务器之间交换数据。

该协议随着时间的推移进行了一些改进，但大多数服务器和客户端都将其实现基于 HTTP 1.1，这是原始 HTTP 1.0 的修订版，向原始协议引入了连接复用功能和分块传输编码。

在典型的 HTTP 流程中，发起操作的客户端通过连接向服务器发送请求并等待服务器的响应。在另一端，服务器从通信通道读取请求，处理请求，并将响应发送回客户端。在下一个图中，我们可以可视化对等体之间交换的请求和响应示例：

