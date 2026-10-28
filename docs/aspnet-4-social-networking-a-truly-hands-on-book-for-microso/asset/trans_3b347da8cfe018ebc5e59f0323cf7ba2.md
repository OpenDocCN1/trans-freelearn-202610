

```csharp
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        dc.MailQueue_Receivings.InsertOnSubmit(MailQueue);
        dc.SubmitChanges();
    }
}
public List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>
    GetMailQueueToProcess()
{
    List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>
        results = new List<pr_MailQueue_SwapReceivingAndWorking_GetWorkingResult>();
    using (FisharooDataContext dc = conn.GetContext())
    {
        results =
        dc.pr_MailQueue_SwapReceivingAndWorking_GetWorking().ToList();
    }
    return results;
}
public void MoveMailQueueWorkingToHistory()
{
    using (FisharooDataContext dc = conn.GetContext())
    {
        dc.pr_MailQueue_MoveWorkingToHistory();
    }
}
```

## 服务

在存储库和数据库就绪之后，我们需要创建一个新的服务。我们已经有一个通过SMTP服务器直接发送邮件的EmailService。但我们需要的是一种可以替代EmailService的新服务——DBMailService。该服务需要符合我们的IEmailService接口，这样我们的应用程序就无需知道邮件是如何发送的。

回想一下，我们的EmailService只有一个方法——Send()。我们将有一个Send()方法，将邮件存入新的EmailRepository。但我们的DBMailService还需要一种让邮件处理器处理邮件的方式。所以我们需要第二个方法ProcessEmails()。

```csharp
//Fisharoo/BusinessLogic/EmailService.cs
public void Send(MailMessage Message)
{
```

另一方面，ProcessEmails()方法稍微复杂一些。首先也是最重要的一点是，这个方法应该一次只运行一个。我们不希望这个方法被任何东西访问，所以需要锁定它。接下来，我们需要获取要处理的邮件列表。然后遍历每封邮件。注意，在遍历邮件消息时，我们正在对每条消息进行反序列化（稍后我们将介绍这一点）。然后我们连接到邮件服务器并发送邮件。

> 与其连接到IIS SMTP服务器或其他非批量发送应用程序，不如连接到PMTA之类的批量邮件投递系统。这些系统会使这个过程更快，因为它们像数据库一样接收邮件，并在接收后处理邮件。IIS SMTP会尝试立即发送，这可能会导致任何尝试发送邮件的应用程序出现延迟。

由于我们向DBMailService添加了一个新方法，我们还需要更新IEmailService接口和EmailService类（以便在所有情况下都能通过MEF获取DBMailService和EmailService）。为此，请为EmailService添加一个针对ProcessEmails()方法的新方法。此方法将抛出一个错误，因为我们不打算实现它。

```csharp
public void ProcessEmails()
{
    throw (new Exception("ProcessEmails is not implemented by this class!"));
}
```

## 序列化邮件

开始本节时，我们得先发点牢骚！我们试图通过队列进程发送MailMessage。这意味着我们需要能够序列化MailMessage。我们认为很多人需要类似的功能，要么将其塞入数据库，要么放入MSMQ，要么放到文件系统。然而MailMessage不可序列化！

这个问题有两种方法。我们可以深入MailMessage的序列化过程，指定MailMessage中每个对象的序列化方式。或者我们可以伪造序列化。深入研究并重写序列化本身就可以写成一章。所以我们打算伪造它。

为了伪造这个过程，我们创建了一个名为MyMailMessage的类，其中包含一个子类MailAddress。这包含了我们目前在邮件中需要表示的所有特性。我们避开了标题、附件等。目前，我们只关心发件人、收件人、主题、正文以及其他一些内容。

```csharp
//Fisharoo/BusinessLogic/MailMessageService.cs
[Serializable]
public class MyMailMessage
{
    public MailAddress[] Bcc { get; set; }
    //refer to actual file for complete code
    public MailAddress Sender { get; set; }
    public string Subject { get; set; }
    [Serializable]
    public class MailAddress
    {
        public string Address { get; set; }
        public string DisplayName { get; set; }
    }
}
```

请注意，它被标记为**可序列化**！

有了这个基础，我们就能创建一些方法，有效地在MailMessage类和新MyMailMessage对象之间来回传输数据。

```csharp
//Fisharoo/BusinessLogic/MailMessageService.cs
private static MailAddressCollection ConvertMyMailAddressesToMailAddresses(List<MyMailMessage.MailAddress> MyMailAddresses)
{
    MailAddressCollection mac = new MailAddressCollection();
    foreach(var a in MyMailAddresses)
    {
        mac.Add(ConvertMyMailAddressToMailAddress(a));
    }
    return mac;
}
private static MailAddress ConvertMyMailAddressToMailAddress(MyMailMessage.MailAddress MyMailAddress)
{
    MailAddress ma = null;
    if(MyMailAddress != null && MyMailAddress.Address != null &&
       MyMailAddress.DisplayName != null)
        ma = new MailAddress(MyMailAddress.Address,
                             MyMailAddress.DisplayName);
    return ma;
}
```

最后，我们还有Serialize()和Deserialize()方法，它们借助辅助方法来操作MailMessage。

作为这个类的一个很好的副产品，我们还创建了SerializeEncrypted()和DeserializeEncrypted()方法，它们从Serialize()方法获取返回的字符串，并分别使用我们的Cryptography.Encrypt()和Cryptography.Decrypt()方法对其进行处理。

## 连接新的DBMailQueueService

既然我们已经启动并运行了这个新服务，并准备进行测试，我们就可以使用MEF方法将其挂接到现有系统中，而无需更改太多现有代码。

### 队列

现在，我们实际上能够从我们的网站收集邮件并将其存入MailQueue。由于这一新功能的实现对于网站是透明的，所有发送邮件的区域现在都会将其存入我们的队列。

> 请记住，您可以选择对要存储在队列中的电子邮件进行加密。考虑到有些电子邮件会包含用户密码，这或许是个好主意。只是提供一些思考！

### 处理队列

队列中有几封邮件后，我们现在可以专注于构建一个示例处理器。我们将构建一个快速的控制台应用程序来演示这一点。从技术上讲，您可以将该控制台应用程序作为计划任务挂接到Windows服务器上。但对于生产环境，我们建议您将此控制台应用程序转换为Windows服务，后者要可靠得多（只是不太容易演示！）。

首先，我们添加了一个名为MailQueueProcessor的新控制台应用程序项目。我们添加了一个app.config文件。该app.config文件包含Configuration类所期望看到的所有属性的副本，以及一个指向邮件服务器的system.net/mailSettings条目。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <system.net>
        <mailSettings>
            <smtp>
                <network host="www.fisharoo.com" port="25"
                         userName="admin@fisharoo.com" password="tm-es@as"/>
            </smtp>
        </mailSettings>
    </system.net>
    <appSettings>
        <add key="SiteName" value="Fisharoo"/>
        <add key="NumberOfRecordsInPage" value="20"/>
        <add key="NumberOfTagsInCloud" value="30"/>
        <!--
            CloudSortOrder - Possible options include the following:
                Random: randomly sorts the cloud each time
                Ascending: sorts the final value of the tag cloud from small
                to tall
                Descending: sorts the final value of the tag cloud from tall
                to small
        -->
        <add key="CloudSortOrder" value="Random"/>
        <add key="TagCloudSmallestFontSize" value="10"/>
        <add key="TagCloudLargestFontSize" value="30"/>
        <add key="RootURL" value="http://localhost:64810/"/>
        <add key="WebSiteURL" value="http://localhost:64810/"/>
        <add key="AdminSiteURL" value="http://localhost:64948/"/>
        <add key="DefaultCacheDuration_Days" value="0"/>
        <add key="DefaultCacheDuration_Hours" value="0"/>
        <add key="DefaultCacheDuration_Minutes" value="15"/>
        <add key="ToEmailAddress" value="website@fisharoo.com"/>
        <add key="FromEmailAddress" value="website@fisharoo.com"/>
    </appSettings>
</configuration>
```

