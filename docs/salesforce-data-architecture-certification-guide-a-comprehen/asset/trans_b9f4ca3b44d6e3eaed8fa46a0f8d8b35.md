
```apex
public void finish(Database.BatchableContext info){}
}
```

`start` 方法用于定义查询。返回的 `Database.QueryLocator` 用于在指定的批大小中有效地检索查询结果（如果未指定，则为 200）。

`execute` 方法用于定义 Apex 类逻辑的大部分内容，并处理在该记录批次中返回的记录。

`finish` 方法用于任何后处理逻辑。我在夜间批处理作业类定义中使用它来调用一封电子邮件，汇总作业的统计信息（处理的总记录数、成功和失败）。

要调用批处理 Apex 类，必须对其进行调度。这可以在 Developer Console 中作为匿名 Apex 执行来完成：

```apex
Id batchJobId = Database.executeBatch(new BatchClass(), 150);
```

在此示例中，我们的批处理 Apex 类名为 `BatchClass`，我们以每次 150 条记录的批大小执行它（因此每次批处理都会由批处理类的 `execute` 方法处理 150 条记录）。

作为示例，让我们编写一个批处理类，返回我们 Salesforce 实例中今天创建的所有 Account 记录，并为每个记录附加短语“processed”。然后，我们将使用默认批大小 200 进行调用：

```apex
public class appendAccountNameBatch implements Database.Batchable {
    public Database.QueryLocator start(Database.BatchableContext info) {
        return Database.getQueryLocator('SELECT Id, Name FROM Account WHERE CreatedDate = TODAY');
    }

    public void execute(Database.BatchableContext info, List<sObject> scope) {
        List<Account> accounts = new List<Account>();
        for (sObject s : scope) {
            Account act = (Account)s;
            act.Name = act.Name + ' processed';
            accounts.add(act);
        }
        update accounts;
    }

    public void finish(Database.BatchableContext info) {
        // Do nothing
    }
}
```

该类将按如下方式调用：

```apex
Id batchJobId = Database.executeBatch(new appendAccountNameBatch());
```

Batch Apex 作业可以在 Salesforce Setup 中通过在快速查找框中输入 Apex Jobs 来查看（并停止）。如果作业为 **Completed**，则表示它已无错误运行。状态为 **Aborted** 或 **Failed** 的作业需要调查，因为它们未成功执行。

完全可以按计划安排 Batch Apex 作业运行，例如每晚运行一次。为此，您需要为批处理 Apex 类创建一个包装类，该类实现 Schedulable 接口，然后根据使用类似 Unix 的 cron 格式定义的计划安排该类执行。

> **重要说明**
>
> **Unix** cron 格式用于指定可调度作业的运行频率。它源自 Unix 操作系统上的 cron 实用程序。它遵循六个值的日/时间格式（年份可选为第七个）。它表示为字符串，例如 `0 0 17 * * ?`，表示每天下午 5 点执行。有关 cron 字符串格式的更多信息，请参阅 `https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_scheduler.htm`。

例如，让我们将 appendAccountNameBatch 类包装起来以允许其被调度：

```apex
public class scheduledAppendAccountNameBatch implements Schedulable {
    public void execute(SchedulableContext sc) {
        appendAccountNameBatch b = new appendAccountNameBatch();
        // your batch class
        Database.executeBatch(b);
    }
}
```

然后，我们可以使用匿名 Apex 调用此可调度类：

```apex
System.schedule('Nightly scheduledAppendAccountNameBatch Job', '0 0 1 * * ?', new scheduledAppendAccountNameBatch());
```

这将导致我们的批处理类每晚凌晨 1 点运行。通过了解 Apex 代码如何被调用以及记录批处理如何帮助我们在 governor limits 内工作，我们可以有效地使用 Apex 代码处理大量记录。

现在我们掌握了 Batch Apex，让我们探索使用 Lightning Platform 上的 Apex 进行异步操作。

## Asynchronous Apex

Apex 代码可以使用批处理类以异步方式运行，并随后排队或调度。Apex 代码也可以通过在方法上使用 `@future` 注解来编写以在后台运行。所有 Apex Web 服务调用都是 `@future` 方法，具有 `callout=true` 参数，即它们被强制异步运行，在后台运行，用户界面操作不会因其调用而被阻塞。

另一种思考异步 Apex 的方式是它本质上在自己的时间执行。使用 `@future` 方法的好处是 governor limits 更高，这意味着 SOQL 查询大小和堆大小限制更宽松，因为执行是在自己的时间、在自己的系统线程上进行。

编写使用 `@future` 注解的 Apex 方法时，它们必须是静态的，并且只能返回 void 返回类型——即不返回任何内容。

下面是一个最基本的异步 Apex 类示例：

```apex
public class asyncClass {
    @future
    public static void anAsyncMethod() { }
}
```

现在我们了解了异步 Apex，让我们总结一下本章所涵盖的内容。

## Summary

在本章中，我们探讨了 Salesforce 平台在数据操作方面的更具编程性的元素。我们在查看 Bulk API 之前先查看了 REST 和 SOAP API。然后，我们将注意力转向 Apex Database 类操作、Batch Apex，以及使用 `@future` 注解的异步 Apex。理解了这些概念后，您作为 Salesforce 数据架构师，现在已熟练掌握可对解决方案状态产生影响的编程行为的决策。

在下一章中，我们将通过实际示例探索更多关于如何缓解 **Large Data Volume (LDV)** 问题的概念，以及如何加载大量数据。

## Practice questions

通过回答以下问题来测试您对本章所涵盖主题的了解：

1. 适合 SOAP API 请求的唯一数据格式是什么？
2. Bulk API 用于哪两种作业类型？
3. 用于描述可由调用应用程序导入的 SOAP API 提供的操作的资源类型是什么？
4. Bulk API Query 操作尝试多少次重试？
5. 运行 Bulk API Ingest 操作时记录被分成什么批大小？
6. Bulk API Ingest 作业支持哪些操作？
7. 编写 Apex 批处理类时扩展了什么接口？
8. 异步 Apex 方法使用什么注解？
9. Apex `insert` 语句，后跟特定对象类型的记录列表，是在处理 Salesforce 数据库时哪种类型方法的示例？
10. Apex `Database` 类的 insert/update/upsert/delete 方法有一个布尔参数，它的作用是什么？

## Answers

以下是前面问题的答案。您能正确回答多少个？在此检查您对前面问题的答案：

1. XML
2. Ingest and Query
3. WSDL
4. 15
5. 10,000
6. Insert, update, upsert, delete
7. Database.Batchable
8. @future
9. A DML statement
10. Determine whether to allow partial processing of records

## Further reading

有关本章所涵盖内容的更多信息，请参阅以下资源：

- *OAuth 2.0 with Salesforce*: https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_oauth_and_connected_apps.htm
- *Batch API*: https://developer.salesforce.com/docs/atlas.en-us.api_asynch.meta/api_asynch/asynch_api_intro.htm
- *Bulk API 2.0 – How requests are processed*: https://developer.salesforce.com/docs/atlas.en-us.api_asynch.meta/api_asynch/how_requests_are_processed.htm
- *Batch Apex*: https://developer.salesforce.com/docs/atlas.en-us.222.0.apexcode.meta/apexcode/apex_batch_interface.htm
- *Executing Batch Apex*: https://help.salesforce.com/s/articleView?id=000328480&type=1
- *The Apex Batchable Interface*: https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_interface_database_batchable.htm#apex_Database_Batchable_execute
- *The Apex Schedulable class*: https://developer.salesforce.com/docs/atlas.en-us.222.0.apexcode.meta/apexcode/apex_scheduler.htm
- *Future methods in Apex*: https://developer.salesforce.com/docs/atlas.en-us.222.0.apexcode.meta/apexcode/apex_invoking_future_methods.htm
- *The Apex Database Class*: https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_database.htm

