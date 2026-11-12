
Force.com 中的异步处理: https://developer.salesforce.com/page/Asynchronous_Processing_in_Force_com.

> 可以通过 Apex Scheduler 的 execute 方法在异步上下文中执行代码。这还提供了增强的控制器限制。根据所涉及的处理，您可能会选择从 execute 方法启动 Batch Apex 作业。否则，如果您需要一次性的计划执行，可以使用 Database.scheduleBatch 方法。

## @future

考虑在使用 @future 作业时遵循以下实施准则：

- 避免向异步队列中添加大量 @future 作业（请考虑 Batch Apex）。
- 方法必须快速执行，否则可能会面临平台限流，这会延迟执行。
- 实现起来更简单，因为只需添加方法注解。
- 通常，出队并执行工作所需的时间少于 Batch Apex。
- 您可以通过方法注解表达特定控制器的增加，以根据您的需求自定义控制器限制。
- 参数类型仅限于简单类型和列表。
- 传入列表参数时要小心，确保使用大列表进行容量测试。
- 无法跟踪执行，因为未返回 Apex 作业 ID。
- 从 Apex 触发器执行时应谨慎考虑；@future 方法应进行批量处理，如果存在批量数据加载的可能性，则更有可能触及 @future 作业的每用户每日限制。
- 您无法启动另一个 @future、Batch Apex 或 Queueables 作业。如果您的打包代码可以由订阅者组织中的代码（例如开发者 X 编写的代码）调用，则这尤其成问题。如果他们有自己的 @future 方法间接调用您对该功能的使用，将会发生异常。因此，请尽量避免从打包的 Apex 触发器上下文中使用这些功能。

> 我推荐使用 Queueables 而非 @future，因为它们在可传递信息类型方面具有更多灵活性，并且提交后更容易跟踪。

## Queueables

考虑在使用 Queueables 时遵循以下实施准则：

- 避免向异步队列中添加大量作业（请考虑 Batch Apex）。
- 方法必须快速执行，否则可能会面临平台限流，这会延迟执行。
- 实现 `Queueable` 接口和 execute 方法。然后，使用 `System.enqueueJob` 方法启动作业：
  `public void execute(QueueableContext context)`
- 实现 `Queueable` 的对象可以包含表示要执行的工作的可序列化状态。这可以是复杂的列表、映射或您自己的 Apex 类型。
- 通常，出队并执行工作所需的时间少于 Batch Apex。
- 处理列表时要小心，确保使用大容量列表进行容量测试，以确保作业能够用最大数据量完成。
- 您可以跟踪执行，因为返回了 **Apex 作业 ID**。
- 从 Apex 触发器执行时应谨慎考虑。使用 `Queueables` 应进行批量处理，如果存在批量数据加载的可能性，则更有可能触及 @future 作业的每用户每日限制。
- 您可以在同步上下文中启动最多 50 个 `queueables` 作业。
- 您只能在现有的 `queueable` 作业中启动一个 `queueable` 作业；这称为链式调用。链的深度不限，但随着深度增加，平台会减慢作业之间的间隔。此外，开发者版组织的深度限制为 5。
- 如果代码尝试在现有 `queueable` 上下文中发出多个 `queueable` 作业，将发生错误。如果您的打包代码可以由订阅者组织中的代码（例如开发者 X 编写的代码）调用，则这尤其成问题。如果他们有自己的异步方法间接调用您对该功能的使用，将会发生异常。因此，请尽量避免从打包的 Apex 触发器上下文中使用这些功能。
- 可以同时运行的 queueables 作业比 Batch Apex 更多。确切限制未记录。但是，请务必避免淹没系统，通过实施控制链式调用和并发性的逻辑来实现。

## Batch Apex

在我们的场景中，加载到 Salesforce 中的 **Race Data** 记录默认不会将自身与适用的 **Contestant** 记录关联。如前所述，**Contestant** 记录具有一个 **Race Data Id** 字段，该字段包含由 **Year**、**Race Name** 和 **Driver Id** 字段拼接而成的 Apex 计算的唯一值。

以下 Batch Apex 作业旨在处理所有 **Race Data** 记录，计算复合 **Contestant Race Data Id** 字段，并将记录与相关 **Contestant** 记录关联更新。以下示例突出了在此上下文中 Selector 和 Service 层模式的几个其他用法：

- `RaceDataSelector` 类用于提供所需的 `QueryLocator` 方法，将 SOQL 逻辑（在此示例中虽简单）封装在 Selector 层中。

- `RaceService.processData` 方法（添加以支持此作业）接收记录的 ID，而不是传递给 execute 方法中的 `scope` 参数的记录。这种通用方法避免了对记录数据过时的任何担忧，因为 Batch Apex 在作业开始时序列化了所有记录数据。

- 按照标准做法，Service 层不处理异常；这些异常抛给调用者并由调用者适当处理。在此示例中，使用新的 `LogService` 类捕获抛出的异常并将其记录到自定义对象中，利用 Apex 作业 ID 作为区分其他作业日志条目的手段。请注意，`LogService` 的使用仅用于说明；它未在本章示例代码中实现。

- 在 `finish` 方法中，新的 `NotificationService` 封装逻辑，能够通知最终用户（可能通过电子邮件、任务或 Chatter，如前所述）作业结果，并传递 Apex 作业 ID，以便它可以利用存储在日志自定义对象和/或 `ApexAsyncJob` 中的信息为用户形成合适的消息。同样，这仅用于说明。

以下代码显示了处理赛车数据记录的 Batch Apex 作业的实现：

```java
public class ProcessRaceDataJob
implements Database.Batchable<SObject> {
public Database.QueryLocator start(
Database.BatchableContext ctx) {
return
RaceDataSelector.newInstance().selectAllQueryLocator();
}

public void execute(
Database.BatchableContext ctx, List<RaceData__c> scope) {
try {
Set<Id> raceDataIds =
new Map<Id, SObject>(scope).keySet();
RaceService.processData(raceDataIds);
}
catch (Exception e) {
LogService.log(ctx.getJobId(), e);
}
}

public void finish(Database.BatchableContext ctx) {
NotificationService.notify(
ctx.getJobId(), 'Process Race Data');
}
}
```

请注意，即使启动 Apex 作业的代码也在 Service 方法中实现；这允许封装某些并发性和配置的预检查。要运行此作业，已在 Race Data 对象上放置了一个列表视图自定义按钮以启动该过程。控制器调用 Service 方法以按如下方式启动作业：

```
Id jobId = RaceService.runProcessDataJob();
```

考虑在使用 Batch Apex 时遵循以下实施准则：

- 组织中最多可运行 5 个活动 Apex 作业。
- **Apex Flex Queue**（可在 **Setup** 下使用）最多可排队 100 个作业。平台会在作业完成时自动从队列中删除项目。您也可以通过 UI 或 API 重新排序队列中的项目。队列中的作业具有特殊的保持状态。

